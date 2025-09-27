# ReputationBook – Public Complaints

## List Complaints (public)

### Endpoint

```
GET /api/v1/complaints
```

Returns the published complaints for the current platform (enforced by the `platform` middleware). Results can be paginated or limited through query parameters.

### Authentication

None. Only the `X-PUBLIC-KEY` header is required to identify the platform.

### Headers

| Header | Type | Required | Description | Default/Values |
| ------ | ---- | -------- | ----------- | -------------- |
| `X-PUBLIC-KEY` | string | Yes | Public key for the consumer platform. | — |
| `Accept-Language` | string | Recommended | Locale (`pt-BR`, `en`, `es`) used to fetch titles/texts when available. | — |

### Query Parameters

| Parameter | Type | Required | Description | Default/Values |
| --------- | ---- | -------- | ----------- | -------------- |
| `page` | integer | No | Current page when paginating. | `1` |
| `per_page` | integer | No | Page size. | `15` |
| `no_paginate` | boolean | No | When `true`, returns a flat list limited by `limit`. | `false` |
| `limit` | integer | No | Maximum items when `no_paginate=true`. | `15` |
| `status` | string | No | Filters by status (`open`, `in_progress`, etc.). | — |
| `company` | string | No | Accepts ID, UUID, slug, or the helpers `email:` / `url:`. | — |
| `title` | string | No | Searches by complaint title. | — |
| `complaint` | string | No | Searches inside the complaint body. | — |
| `with_texts` | boolean | No | Includes full texts in the response. | `false` |
| `created_at`, `created_at_period[]` | date / array | No | Filters by creation date (exact or range). | — |
| `purchase_date`, `purchase_date_period[]` | date / array | No | Filters by informed purchase date. | — |
| `month`, `year` | integer | No | Filters by month/year (both required when `month` is present). | — |

> Parameter names are canonical in `snake_case`, but `camelCase`, `kebab-case`, and `CapitalCase` are also accepted.

### Example Response (200)

```json
{
  "data": [
    {
      "uuid": "0d5b1c74-5a92-4fa0-91ba-51f0f0d41ce5",
      "is_public": true,
      "title": {
        "title": "Delayed delivery",
        "slug": "delayed-delivery",
        "language": "en",
        "is_default": true
      },
      "text": "The order arrived 7 days late...",
      "company": {
        "uuid": "f632c350-1f7c-4eee-964d-36c631f31e4a",
        "name": "Sample Store",
        "slug": "sample-store"
      },
      "status": "open",
      "purchase_date": "2025-08-12",
      "created_at": "2025-08-13T17:01:22+00:00"
    }
  ],
  "meta": {
    "current_page": 1,
    "per_page": 15,
    "total": 42
  }
}
```

### Explained JSON Structure

| Field | Type | Description |
| ----- | ---- | ----------- |
| `data[]` | array | List of public complaints. |
| `data[].uuid` | uuid | Complaint identifier. |
| `data[].title` | object | Localized title for the complaint. |
| `data[].text` | string | Snippet of the main text (truncated publicly). |
| `data[].company` | object | Associated company. |
| `data[].status` | string | Current status (`open`, `resolved`, ...). |
| `meta` | object | Pagination metadata when applicable. |

### HTTP Status Codes

- 200: List returned successfully.
- 400: Invalid filters.
- 429: Rate limited.
- 500: Internal error.

---

## Create Complaint

### Endpoint

```
POST /api/v1/complaints
```

Creates a new complaint on behalf of the authenticated user.

### Authentication

Required – `Bearer {token}` via `auth:sanctum` with abilities `backoffice` **and** `domain:reputationbook`.

### Headers

| Header | Type | Required | Description |
| ------ | ---- | -------- | ----------- |
| `Authorization` | string | Yes | Valid `Bearer {token}`. |
| `X-PUBLIC-KEY` | string | Yes | Identifies the platform. |
| `Accept-Language` | string | Recommended | Preferred locale for title/text. |
| `Content-Type` | string | Yes | `application/json`. |

### Request Body (JSON)

| Field | Type | Required | Description |
| ----- | ---- | -------- | ----------- |
| `company_uuid` | uuid | Conditional | Company UUID. Required when `company_id` is absent. |
| `company_id` | integer | Conditional | Internal company ID (mutually exclusive with `company_uuid`). |
| `purchase_date` | date | Yes | Purchase date (must be <= today). |
| `title` | string | Yes | Complaint title. |
| `complaint` | string | Yes | Complaint body. |
| `is_public` | boolean | No | Marks the complaint as public. |
| `invoice_number` | string | No | Invoice reference. |
| `attendants_name` | string | No | Names of the attendants involved. |
| `language` | string | No | Locale for content (`pt-BR`, `en`, etc.). |

### Example Request

```json
{
  "company_uuid": "f632c350-1f7c-4eee-964d-36c631f31e4a",
  "purchase_date": "2025-08-12",
  "title": "Delayed delivery",
  "complaint": "The order was supposed to arrive within 48 hours...",
  "is_public": true,
  "invoice_number": "NF-12345",
  "language": "en"
}
```

### Example Response (201)

```json
{
  "data": {
    "uuid": "31db8d4a-0d0f-4d38-9c6a-c4c0ad9c8d41",
    "is_public": true,
    "platform": {
      "uuid": "c0aec1f4-d5ae-4ffd-8d24-8e55e9a3aca7",
      "domain_area": {
        "uuid": "963e42f1-5c2e-4a86-8866-2e05fcf7f3f9",
        "name": "Consumer Protection",
        "slug": "consumer-protection"
      }
    },
    "title": {
      "title": "Delayed delivery",
      "language": "en",
      "is_default": true
    },
    "texts": [
      {
        "content": "The order was supposed to arrive within 48 hours...",
        "usage": "complaint_text",
        "language": "en",
        "is_default": true
      }
    ],
    "company": {
      "uuid": "f632c350-1f7c-4eee-964d-36c631f31e4a",
      "name": "Sample Store",
      "slug": "sample-store"
    },
    "status": "open",
    "purchase_date": "2025-08-12",
    "invoice_number": "NF-12345",
    "created_at": "2025-08-13T17:01:22+00:00"
  }
}
```

### HTTP Status Codes

- 201: Complaint created.
- 400: Missing required fields or conflicting `company_uuid` / `company_id`.
- 401: Invalid token.
- 403: User lacks abilities.
- 422: Validation errors.

### Notes

- Authorization requires the `create` policy on `Complaint`.
- Additional titles/texts can be added via the `add-text` endpoint.

---

## Retrieve Complaint by UUID

### Endpoint

```
GET /api/v1/complaints/{complaint}
```

Returns a complaint by identifier. The parameter accepts ID, UUID, or slug according to the binding heuristics.

### Authentication

Optional. Authenticated users receive additional fields (full text, user info, invoice, images).

### Path Parameters

| Parameter | Type | Description |
| --------- | ---- | ----------- |
| `complaint` | string | Complaint identifier (ID/UUID/slug). |

### HTTP Status Codes

- 200: Complaint found.
- 404: Complaint not found.

---

## Update Complaint

### Endpoint

```
PUT /api/v1/complaints/{complaint}
```

Updates complaint attributes.

### Authentication

Required – `Bearer {token}` with abilities `backoffice` and `domain:reputationbook`.

### Request Body (JSON)

| Field | Type | Required | Description |
| ----- | ---- | -------- | ----------- |
| `company_uuid` | uuid | No | Updates the company (mutually exclusive with `company_id`). |
| `company_id` | integer | No | Company ID. |
| `purchase_date` | date | No | New purchase date. |
| `title` | string | No | New title (requires `language`). |
| `complaint` | string | No | New complaint body (requires `language`). |
| `language` | string | Conditional | Required when `title` or `complaint` is provided. |
| `is_public` | boolean | No | Updates visibility. |
| `invoice_number` | string | No | Updates invoice info. |

> The payload must include at least one mutable field; otherwise a `400` (`validation.empty_request`) is returned.

### HTTP Status Codes

- 200: Update applied.
- 400: Empty body.
- 401/403: Missing permissions.
- 404: Complaint not found.
- 422: Invalid data.

---

## Update Complaint Status

### Endpoint

```
PATCH /api/v1/complaints/{complaint}/status
```

### Request Body

| Field | Type | Required | Description |
| ----- | ---- | -------- | ----------- |
| `status` | string | Yes | New status (enum `ComplaintStatusEnum`). |

### Notes

- Requires the `update` policy check on the complaint.
- The authenticated user is recorded as the actor for this change.

### HTTP Status Codes

- 200: Status updated.
- 400: Invalid status.
- 401/403: User not allowed.
- 404: Complaint not found.

---

## Delete Complaint

### Endpoint

```
DELETE /api/v1/complaints/{complaint}
```

Removes the complaint and records the authenticated user as the actor.

### Authentication

Required – `Bearer {token}` (`auth:sanctum`) with abilities `backoffice` and `domain:reputationbook`.

### HTTP Status Codes

- 204: Deletion completed.
- 401/403: User lacks `delete` permission.
- 404: Complaint not found.

---

## Add Complementary Text

### Endpoint

```
POST /api/v1/complaints/{complaint}/add-text
```

### Authentication

Required – `Bearer {token}` with abilities `backoffice` and `domain:reputationbook`.

### Request Body (JSON)

| Field | Type | Required | Description |
| ----- | ---- | -------- | ----------- |
| `content` | string | Yes | Additional text (up to 5000 characters). |
| `language` | string | No | Text language (`LanguageEnum`). |
| `usage` | string | No | Specific usage (defaults to `complaint_text`). |

### HTTP Status Codes

- 200: Text attached and resource returned.
- 401/403: User not authorized.
- 404: Complaint not found.
- 422: Invalid content.

### Notes

- The text is added to the `texts` relation; use `all_texts=true` to retrieve every version.

