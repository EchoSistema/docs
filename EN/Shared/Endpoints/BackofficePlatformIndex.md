# Shared – List Platforms

## Endpoint

```
GET /api/v1/backoffice/platforms
```

## Authentication

Required – Bearer {token} with ability `backoffice`

## Headers

| Header     | Type | Required | Description |
| ---------------- | ------ | -------- | ----------- |
| Authorization    | string | When required | `Bearer {token}`. |
| X-PUBLIC-KEY     | string | Yes      | Platform public key. |
| Accept-Language  | string | No       | IETF locale (e.g., `pt-BR`, `en`, `es`). |

## Query Parameters

| Parameter | Type | Required | Description |
| ----------- | ------- | ----------- | ----------- |
| no_paginate | boolean | No          | If `true`, returns all results without pagination. |
| per_page    | integer | No          | Number of records per page (default: 25). |

## Examples

### Request example (curl)

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: en" \
  "https://sandbox.your-domain.com/api/v1/backoffice/platforms?per_page=25"
```

### Response example

```json
{
  "data": [
    {
      "uuid": "9d4e1c2a-3b4c-5d6e-7f8g-9h0i1j2k3l4m",
      "total": {
        "users": 150
      },
      "domain": {
        "uuid": "8c3d0b1a-2e3f-4g5h-6i7j-8k9l0m1n2o3p",
        "name": "E-commerce"
      },
      "name": "Demo Platform",
      "email": "contact@platform.com",
      "open_to_register": true,
      "public_key": "pk_test_123456789",
      "logo": {
        "main": "https://cdn.example.com/logos/main.png",
        "square": "https://cdn.example.com/logos/square.png",
        "rounded": "https://cdn.example.com/logos/rounded.png",
        "rectangular": "https://cdn.example.com/logos/rectangular.png"
      },
      "created_at": "2024-01-15T10:30:00Z",
      "company_uuid": "7b2c9a0b-1d2e-3f4g-5h6i-7j8k9l0m1n2o",
      "slug": "demo-platform",
      "country": "United States",
      "state": "California",
      "city": "San Francisco",
      "company_logos": [
        {
          "main": "https://cdn.example.com/company/main.png"
        },
        {
          "square": "https://cdn.example.com/company/square.png"
        }
      ],
      "full_address": "123 Market St, San Francisco, CA 94102",
      "links": [
        {
          "uuid": "6a1b8c9d-0e1f-2g3h-4i5j-6k7l8m9n0o1p",
          "platform": "facebook",
          "url": "https://facebook.com/platform"
        },
        {
          "uuid": "5a0b7c8d-9e0f-1g2h-3i4j-5k6l7m8n9o0p",
          "platform": "instagram",
          "url": "https://instagram.com/platform"
        }
      ]
    }
  ],
  "links": {
    "first": "https://sandbox.your-domain.com/api/v1/backoffice/platforms?page=1",
    "last": "https://sandbox.your-domain.com/api/v1/backoffice/platforms?page=5",
    "prev": null,
    "next": "https://sandbox.your-domain.com/api/v1/backoffice/platforms?page=2"
  },
  "meta": {
    "current_page": 1,
    "from": 1,
    "last_page": 5,
    "per_page": 25,
    "to": 25,
    "total": 125
  }
}
```

## JSON Structure Explained

| Field | Type | Description |
| ----------- | ------- | ----------- |
| data        | array   | List of platforms |
| data[].uuid | string  | Platform unique identifier |
| data[].total | object  | Platform totals |
| data[].total.users | integer | Total number of users on the platform |
| data[].domain | object  | Domain area information |
| data[].domain.uuid | string  | Domain area unique identifier |
| data[].domain.name | string  | Domain area name |
| data[].name | string  | Platform name |
| data[].email | string  | Platform contact email |
| data[].open_to_register | boolean | Indicates if the platform is open for new registrations |
| data[].public_key | string  | Platform public key |
| data[].logo | object  | Platform logo URLs |
| data[].logo.main | string  | Main logo URL |
| data[].logo.square | string  | Square logo URL |
| data[].logo.rounded | string  | Rounded logo URL |
| data[].logo.rectangular | string  | Rectangular logo URL |
| data[].created_at | string  | Creation date (ISO 8601) |
| data[].company_uuid | string  | Company unique identifier (when available) |
| data[].slug | string  | Company slug (when available) |
| data[].country | string  | Company country (when available) |
| data[].state | string  | Company state (when available) |
| data[].city | string  | Company city (when available) |
| data[].company_logos | array   | Company logos (when available) |
| data[].full_address | string  | Full formatted address (when available) |
| data[].links | array   | Company social media links (when available) |
| data[].links[].uuid | string  | Link unique identifier |
| data[].links[].platform | string  | Social platform name |
| data[].links[].url | string  | Social profile URL |
| links       | object  | Pagination links |
| meta        | object  | Pagination metadata |

## HTTP Status

- 200: OK
- 201: Created
- 400: Bad Request
- 401: Unauthorized
- 403: Forbidden
- 404: Not Found
- 422: Unprocessable Entity
- 429: Too Many Requests
- 500: Internal Server Error

## Errors

```json
{
  "message": "Error message"
}
```

## Notes

- This endpoint returns all platforms registered in the system
- Only users with backoffice permission can access
- Response includes relationships with company, company.logos and company.address
- Includes user count per platform (users_count)
- Ordered by creation date (descending)

## Related

- [Show Platform Details](BackofficePlatformShow.md)
- [List Platform Users](BackofficePlatformUserIndex.md)

## Changelog

- 2025-10-25: Complete documentation update with detailed structure
- 2025-10-16: Initial documentation
