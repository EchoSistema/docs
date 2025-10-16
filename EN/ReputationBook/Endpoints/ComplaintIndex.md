# ReputationBook – List Public Complaints

## Endpoint

```
GET /api/v1/complaints
```

Retrieves a paginated list of public complaints with comprehensive filtering options.

## Authentication

None (public endpoint)

## Headers

| Header           | Type   | Required | Description |
| ---------------- | ------ | -------- | ----------- |
| X-PUBLIC-KEY     | string | Yes      | Platform public key. |
| Accept-Language  | string | No       | IETF locale (e.g., `pt-BR`, `en`, `es`). Determines localization for translatable fields. |

## Parameters

### Query parameters

| Parameter | Type    | Required | Description | Default/Values |
| --------- | ------- | -------- | ----------- | -------------- |
| user      | string  | No       | Filter by user UUID, ID, or name. | - |
| company   | string  | No       | Filter by company UUID, ID, name, email, or URL. Accepts prefixes `email:` and `url:`. | - |
| company_name | string | No | Filter by company name (partial match). | - |
| company_email | string | No | Filter by company email. | - |
| company_url | string | No | Filter by company website URL. | - |
| status    | string  | No       | Filter by complaint status (e.g., `pending`, `in_progress`, `resolved`, `rejected`). | - |
| is_public | boolean | No       | Filter by public visibility. Automatically set to `true` for this endpoint. | true |
| title     | string  | No       | Filter by complaint title (partial match). | - |
| complaint | string  | No       | Filter by complaint text (partial match). | - |
| complaint_language | string | No | Filter complaints by text language code. | - |
| invoice   | string  | No       | Filter by invoice number. | - |
| created_at | date   | No       | Filter by exact creation date. | - |
| created_at_period | array | No | Filter by date range `[start_date, end_date]`. | - |
| purchase_date | date | No | Filter by exact purchase date. | - |
| purchase_date_period | array | No | Filter by purchase date range `[start_date, end_date]`. | - |
| month     | integer | No       | Filter by month (1-12). | - |
| year      | integer | No       | Filter by year (1990-2100). Required if `month` is provided. | - |
| with_texts | boolean | No | Include complaint text content in response. | false |
| per_page  | integer | No       | Items per page for pagination. | 15 |
| no_paginate | boolean | No | When `true`, disables pagination and returns all results (limited by `limit`). | false |
| limit     | integer | No       | Maximum number of results when `no_paginate` is true. | - |

> All filter parameters accept `camelCase`, `kebab-case`, and `snake_case` variants.

## Examples

### Request example (curl)

```bash
curl -X GET \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: en" \
  "https://sandbox.your-domain.com/api/v1/complaints?status=pending&per_page=10"
```

### Response example

```json
{
  "data": [
    {
      "uuid": "9d1234ab-5678-90ef-1234-567890abcdef",
      "user": {
        "uuid": "9a1234ab-5678-90ef-1234-567890abcdef",
        "name": "John Doe",
        "gender": "male"
      },
      "platform": {
        "uuid": "9b1234ab-5678-90ef-1234-567890abcdef",
        "name": "Platform Name",
        "domain_area": {
          "uuid": "9c1234ab-5678-90ef-1234-567890abcdef",
          "name": "Service Area",
          "slug": "service-area"
        }
      },
      "company": {
        "uuid": "9e1234ab-5678-90ef-1234-567890abcdef",
        "name": "Company ABC",
        "slug": "company-abc"
      },
      "titles": [
        {
          "content": "Product quality issue",
          "language": "en",
          "is_default": true
        }
      ],
      "status": "pending",
      "is_public": true,
      "purchase_date": "2025-10-01",
      "created_at": "2025-10-15T14:30:00.000000Z",
      "updated_at": "2025-10-15T14:30:00.000000Z"
    }
  ],
  "links": {
    "first": "https://api.example.com/api/v1/complaints?page=1",
    "last": "https://api.example.com/api/v1/complaints?page=5",
    "prev": null,
    "next": "https://api.example.com/api/v1/complaints?page=2"
  },
  "meta": {
    "current_page": 1,
    "from": 1,
    "last_page": 5,
    "path": "https://api.example.com/api/v1/complaints",
    "per_page": 15,
    "to": 15,
    "total": 72
  }
}
```

## JSON Structure Explained

| Field       | Type    | Description |
| ----------- | ------- | ----------- |
| data[]      | array   | List of complaint objects. |
| data[].uuid | uuid    | Unique identifier for the complaint. |
| data[].user | object  | User who created the complaint. |
| data[].platform | object | Platform where the complaint was registered. |
| data[].company | object | Company being complained about. |
| data[].titles[] | array | Localized complaint titles. |
| data[].status | string | Current complaint status. |
| data[].is_public | boolean | Whether the complaint is publicly visible. |
| data[].purchase_date | date | Date of purchase related to the complaint. |
| data[].created_at | string | Complaint creation timestamp (ISO-8601). |
| data[].updated_at | string | Complaint last update timestamp (ISO-8601). |
| links       | object  | Pagination links. |
| meta        | object  | Pagination metadata. |

## HTTP Status

- 200: OK
- 400: Bad Request
- 401: Unauthorized
- 422: Unprocessable Entity
- 429: Too Many Requests
- 500: Internal Server Error

## Errors

```json
{
  "message": "Validation error",
  "errors": {
    "status": ["The selected status is invalid."]
  }
}
```

## Notes

- This endpoint automatically filters to show only public complaints (`is_public=true`).
- The `titles` array is filtered by the `Accept-Language` header or returns the default title.
- When `with_texts=true`, complaint text content is included in the response.
- Pagination is enabled by default with 15 items per page.
- String filters perform case-insensitive partial matching.

## Related

- [Complaint Show](ComplaintShow.md)
- [Complaint Store](ComplaintStore.md)
- [Complaint Statuses](ComplaintStatus.md)
