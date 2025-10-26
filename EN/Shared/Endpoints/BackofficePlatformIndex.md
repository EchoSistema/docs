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
| Authorization    | string | When required | `Bearer {token}` credential. |
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
      "is_parent": true,
      "domain": {
        "uuid": "8c3d0b1a-2e3f-4g5h-6i7j-8k9l0m1n2o3p",
        "slug": "ecommerce",
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
      "company": {
        "uuid": "7b2c9a0b-1d2e-3f4g-5h6i-7j8k9l0m1n2o",
        "name": "Demo Company Ltd",
        "slug": "demo-company",
        "zipcode": "01310-100",
        "country": "Brazil",
        "state": "São Paulo",
        "city": "São Paulo",
        "company_logos": [
          {
            "main_logo": "https://cdn.example.com/company/main.png"
          },
          {
            "square_logo": "https://cdn.example.com/company/square.png"
          }
        ],
        "full_address": "Av. Paulista, 1000 - Bela Vista, São Paulo - SP, 01310-100",
        "links": [
          {
            "uuid": "6a1b8c9d-0e1f-2g3h-4i5j-6k7l8m9n0o1p",
            "platform": "Facebook",
            "url": "https://facebook.com/platform"
          },
          {
            "uuid": "5a0b7c8d-9e0f-1g2h-3i4j-5k6l7m8n9o0p",
            "platform": "Instagram",
            "url": "https://instagram.com/platform"
          }
        ]
      }
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

### Main Fields

| Field | Type | Description |
| ----------- | ------- | ----------- |
| data        | array   | List of platforms |
| data[].uuid | string  | Platform unique identifier |
| data[].total | object  | Platform totals (conditional - only if there are users) |
| data[].total.users | integer | Total users in the platform |
| data[].is_parent | boolean | Indicates if the platform is a parent platform |
| data[].domain | object  | Domain area information |
| data[].domain.uuid | string  | Domain area unique identifier |
| data[].domain.slug | string  | Domain area slug |
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

### Company Fields (conditional)

The fields below are grouped within the `company` object and only appear when the platform has an associated company:

| Field | Type | Description |
| ----------- | ------- | ----------- |
| data[].company | object  | Company data (conditional) |
| data[].company.uuid | string  | Company unique identifier |
| data[].company.name | string  | Company name |
| data[].company.slug | string  | Company slug |
| data[].company.zipcode | string\|null  | Company zip code |
| data[].company.country | string\|null  | Company country |
| data[].company.state | string\|null  | Company state |
| data[].company.city | string\|null  | Company city |
| data[].company.company_logos | array   | Company logos (empty array if not loaded) |
| data[].company.full_address | string\|null  | Full formatted address |
| data[].company.links | array   | Company social media links (empty array if not loaded) |
| data[].company.links[].uuid | string  | Link unique identifier |
| data[].company.links[].platform | string  | Social platform name |
| data[].company.links[].url | string  | Social profile URL |

### Pagination Metadata

| Field | Type | Description |
| ----------- | ------- | ----------- |
| links       | object  | Page navigation links |
| links.first | string  | First page URL |
| links.last | string  | Last page URL |
| links.prev | string\|null  | Previous page URL (null if first page) |
| links.next | string\|null  | Next page URL (null if last page) |
| meta        | object  | Pagination metadata |
| meta.current_page | integer | Current page number |
| meta.from | integer | First record number on page |
| meta.last_page | integer | Last page number |
| meta.per_page | integer | Records per page |
| meta.to | integer | Last record number on page |
| meta.total | integer | Total records |

## Loaded Relationships

This endpoint automatically loads the following relationships:

- `domainArea` - Platform domain area
- `company` - Associated company
- `company.logos` - Company logos
- `company.address` - Company address (used for country, state, city)
- `company.socialMedias` - Company social media

## Counters

- `users_count` - Total users registered on the platform

## HTTP Status

- 200: OK
- 401: Unauthenticated
- 403: Forbidden (without `index.all` permission)
- 422: Validation error
- 429: Rate limit exceeded
- 500: Internal error

## Errors

```json
{
  "message": "Error message"
}
```

## Notes

- This endpoint returns all platforms registered in the system
- Only users with `index.all` permission can access
- Sorted by creation date (descending) - most recent appear first
- The `total.users` field only appears when the platform has registered users
- The `company` object only appears when the platform has an associated company
- The `company_logos` and `links` arrays return empty array when relationships are not loaded
- All logo URLs are complete and ready to use
- Dates follow ISO 8601 format
- Default pagination returns 25 records per page
- Use `no_paginate=true` to get all results without pagination

## Related

- [Show Platform Details](BackofficePlatformShow.md)
- [List Platform Users](BackofficePlatformUserIndex.md)

## Changelog

- 2025-10-25: Complete update with new company structure and additional fields
- 2025-10-16: Initial documentation
