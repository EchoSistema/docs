# ReputationBook – List Complaints (Backoffice)

## Endpoint

```
GET /api/v1/backoffice/complaints
```

## Description

Lists all system complaints with advanced filters. Exclusive endpoint for backoffice administrators.

## Authentication

Required – Bearer {token} with `backoffice` ability

## Headers

| Header           | Type   | Required | Description |
| ---------------- | ------ | -------- | ----------- |
| Authorization    | string | Yes      | `Bearer {token}` credential. |
| X-PUBLIC-KEY     | string | Yes      | Platform public key. |
| Accept-Language  | string | No       | IETF locale (e.g., `pt-BR`, `en`, `es`). |

## Query Parameters

| Parameter      | Type    | Required | Description |
| -------------- | ------- | -------- | ----------- |
| platform_id    | integer | No       | Filter by platform ID |
| company_id     | integer | No       | Filter by company ID |
| user_id        | integer | No       | Filter by user ID |
| status         | string  | No       | Filter by complaint status |
| is_public      | boolean | No       | Filter by public visibility |
| language       | string  | No       | Language for titles (default: pt-BR) |
| no_paginate    | boolean | No       | If `true`, returns all results without pagination |
| per_page       | integer | No       | Records per page (default: 15) |
| limit          | integer | No       | Result limit when `no_paginate=true` |

## Example Response

```json
{
  "data": [
    {
      "uuid": "9d4e1c2a-3b4c-5d6e-7f8g-9h0i1j2k3l4m",
      "is_public": true,
      "status": "pending",
      "purchase_date": "2024-01-10T00:00:00Z",
      "invoice_number": "INV-2024-001",
      "created_at": "2024-01-15T10:30:00Z",
      "user": {
        "uuid": "8c3d0b1a-2e3f-4g5h-6i7j-8k9l0m1n2o3p",
        "name": "John Doe",
        "email": "john.doe@example.com",
        "gender": "Male"
      },
      "platform": {
        "uuid": "7b2c9a0b-1d2e-3f4g-5h6i-7j8k9l0m1n2o",
        "name": "Demo Platform",
        "domain_area": {
          "uuid": "6a1b8c9d-0e1f-2g3h-4i5j-6k7l8m9n0o1p",
          "name": "E-commerce",
          "slug": "ecommerce"
        }
      },
      "title": {
        "title": "Defective product",
        "slug": "defective-product",
        "language": "en",
        "is_default": true
      }
    }
  ]
}
```

## Loaded Relations

- `user` - User who created the complaint
- `platform` - Platform where registered
- `platform.domainArea` - Platform domain area
- `company` - Complained company (when available)
- `titles` - Multilingual titles

## Available Filters

- **platform_id**: Filter by specific platform
- **company_id**: Filter by specific company
- **user_id**: Filter by specific user
- **status**: Filter by status
- **is_public**: Filter by public visibility
- **language**: Defines language for returned titles

## HTTP Status

- 200: OK
- 401: Unauthorized
- 403: Forbidden (no `backoffice` ability)
- 422: Validation error
- 429: Too many requests
- 500: Internal error

## Notes

- Requires `backoffice` ability in authentication token
- Default sort: most recent first
- Default pagination returns 15 records per page
- Use `no_paginate=true` with `limit` to get all results
- All emails and sensitive data are exposed (administrative endpoint)
- Dates follow ISO 8601 format

## Related

- [View Complaint Details](BackofficeComplaintShow.md)

## Changelog

- 2025-10-26: Initial backoffice endpoint documentation
