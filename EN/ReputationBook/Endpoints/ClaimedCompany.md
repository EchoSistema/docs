# ReputationBook – Claimed Company

## Fetch Company (internal)

### Endpoint

```
GET /api/v1/complaint-book/claimed-company/{company}
```

Returns full details of a claimed company (including complaints, documents, and media), respecting the permissions of the authenticated collaborator.

### Authentication

Required – `Bearer {token}` with abilities `backoffice` and `domain:reputationbook`.

### Path Parameters

| Parameter | Type | Description |
| --------- | ---- | ----------- |
| `company` | string | Company identifier: ID, UUID, or slug. |

### Response

Based on `ClaimedCompanyResource`, including:

- General data (`uuid`, `name`, `assumed_name`, `rating`).
- Complementary information (`addresses`, `contacts`, `social_medias`, `images`, `videos`).
- Fiscal documents (`fiscal_documents`) when `public=false`.
- Related complaints (`complaints`).
- Creator metadata when authenticated.

### HTTP Status Codes

- 200: Company found.
- 403: Platform outside coverage area or collaborator lacks permission.
- 404: Company not found / outside coverage.

---

## Fetch Company (public)

### Endpoint

```
GET /api/v1/complaint-book/claimed-company/{company}/info
```

Returns a public-friendly summary of the company.

### Authentication

None. Platform middleware still requires `X-PUBLIC-KEY`.

### Notes

- Uses the same resource but with the `public=true` flag, omitting fiscal documents and internal complaints.

---

## Check RUC

### Endpoint

```
GET /api/v1/complaint-book/claimed-company/ruc/{param}
```

Looks up the fiscal document of type RUC. If the company already exists, returns its data; otherwise, it queries the external service.

### Path Parameter

`param` should contain the RUC (numbers with or without DV). The controller normalizes the value before querying.

### Response

- Existing: returns `ClaimedCompanyResource` with the additional payload `{ existent: true }`.
- Not found: triggers the scraper (`http://ruc.local:3000`) and returns `data` with the collected fields plus `existent: false`.

### HTTP Status Codes

- 200: Result found (cache or external service).
- 404: No data for the provided RUC.
- 503: External service unavailable.

---

## Create Company

### Endpoint

```
POST /api/v1/complaint-book/claimed-company
```

Creates a company associated with the current platform.

### Authentication

Required – `Bearer {token}` with abilities `backoffice` and `domain:reputationbook`.

### Request Body (summary of `CompanyStoreRequest`)

| Field | Type | Required | Description |
| ----- | ---- | -------- | ----------- |
| `name` | string | Yes | Corporate or trade name. |
| `assumed_name` | string | No | Alternate trade name. |
| `raw.country` / `raw.state` / `raw.city` | string | Yes | Basic location. |
| `raw.address.*` | object | No | Address components (street lines, ZIP code, etc.). |
| `raw.email` | string | Conditional | Contact email (required if `raw.phone` is missing). |
| `raw.phone.country_code`, `raw.phone.number` | string | Conditional | Main phone (required if `raw.email` is missing). |
| `raw.document.type` | string | Conditional | Fiscal document type (`FiscalDocumentTypeEnum`). |
| `raw.document.value` | string | Conditional | Document number. |
| `contacts[]` | array | No | Additional contacts (type + value). |
| `social_medias[]` | array | No | Social network links. |
| `raw` | object | No | May include `geolocation`, `website`, `google.place_id`, etc. |

### Response

Returns `ClaimedCompanyResource` with the full data set.

### HTTP Status Codes

- 201: Company created.
- 400/422: Invalid or incomplete payload.
- 401/403: Missing permission.

---

## Update Company

### Endpoint

```
PUT /api/v1/complaint-book/claimed-company/{company}
```

Updates company details, contacts, or address.

### Request Body (summary of `CompanyUpdateRequest`)

| Field | Type | Required | Description |
| ----- | ---- | -------- | ----------- |
| `type` | string | Yes | `company` (general data) or `address` (address update). Determined automatically. |
| `name`, `assumed_name`, `email`, `website` | string | Conditional | General fields (required when `type=company`). |
| `document.*` / `documents[]` | array | No | Updates fiscal documents. |
| `contacts[]` | array | No | Updates contacts (type + phone/country code). |
| `social_medias[]`, `videos[]` | array | No | Official channels (platform + URL). |
| `address.*` | object | Conditional | Address data (required when `type=address`), including country/state/city IDs. |

### HTTP Status Codes

- 200: Update applied.
- 400/422: Invalid data or unsupported type.
- 404: Company not found.

---

## Upload Company Image

### Endpoint

```
POST /api/v1/complaint-book/claimed-company/{company}/images
```

Stores or replaces company images (logos, marketing assets, etc.).

### Authentication

Required – `Bearer {token}` with abilities `backoffice` and `domain:reputationbook`.

### Request Body

Follows `ImageStoreRequest` (see [user profile](ComplaintBookUsers.md)). Provide `usage` (`logo`, `banner`, ...) and one of `image_url`, `image_encoded`, or `image_file`.

### Response

Returns `ImageResource` with metadata about the stored image.

### HTTP Status Codes

- 201: Image saved.
- 400/422: Invalid upload.
- 404: Company not found.

