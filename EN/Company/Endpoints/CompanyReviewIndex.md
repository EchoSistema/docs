# Company – List Company Reviews

## Endpoint

```
GET /api/v1/company/reviews
GET /api/v1/company/reviews/my
```

## Authentication

- `/api/v1/company/reviews` - None
- `/api/v1/company/reviews/my` - Required - Bearer {token} with `backoffice` ability

## Headers

| Header           | Type   | Required | Description |
| ---------------- | ------ | -------- | ----------- |
| Authorization    | string | For `/my` route | `Bearer {token}`. |
| X-PUBLIC-KEY     | string | Yes      | Platform public key. |
| Accept-Language  | string | No       | IETF locale (e.g., `pt-BR`, `en`, `es`). |

## Parameters

### Query parameters

| Parameter | Type    | Required | Description | Default/Values |
| --------- | ------- | -------- | ----------- | -------------- |
| is_public | boolean | No | Filter public reviews only. Auto-set to true if not authenticated. | - |
| company | string | No | Filter by company UUID or slug. | - |
| user | string | No | Filter by user UUID. | - |
| keywords | string | No | Search in review text. | - |
| rating | integer | No | Filter by rating value. | 1-5 |
| created_at | date | No | Filter by creation date. | ISO 8601 date |
| interval | string | No | Time interval filter. | hour, day, week, month, year |
| period | object | No | Date range filter. | - |
| period.from | date | No | Start date of period. | ISO 8601 date |
| period.to | date | No | End date of period. | ISO 8601 date |
| with_images | boolean | No | Filter reviews with images only. | - |
| sort_by | string | No | Field to sort by. | created_at |
| order_by | string | No | Sort direction. | desc (ASC, DESC) |
| page | integer | No | Page number for pagination. | 1 |
| per_page | integer | No | Items per page. | 25 |
| no_paginate | boolean | No | Disable pagination. | false |
| limit | integer | No | Limit results when `no_paginate` is true. | - |

> Parameter names are documented in `snake_case`. The endpoint accepts variations (`camelCase`, `kebab-case`, `CapitalCase`).

## Examples

### Request example (curl)

```bash
curl -X GET \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: en" \
  "https://sandbox.your-domain.com/api/v1/company/reviews?rating=5&per_page=10"
```

### Response example

```json
{
  "data": [
    {
      "id": 1,
      "uuid": "123e4567-e89b-12d3-a456-426614174000",
      "review": "Excellent service and quality products.",
      "rating": 5,
      "is_public": true,
      "created_at": "2025-01-15T10:30:00.000000Z",
      "company": {
        "id": 1,
        "uuid": "223e4567-e89b-12d3-a456-426614174000",
        "name": "Sample Company",
        "slug": "sample-company",
        "rating": {
          "average": 4.5,
          "total": 120
        },
        "logos": [
          {
            "url": "https://example.com/logo.png",
            "usage": "logo"
          }
        ]
      }
    }
  ],
  "links": {
    "first": "https://sandbox.your-domain.com/api/v1/company/reviews?page=1",
    "last": "https://sandbox.your-domain.com/api/v1/company/reviews?page=10",
    "prev": null,
    "next": "https://sandbox.your-domain.com/api/v1/company/reviews?page=2"
  },
  "meta": {
    "current_page": 1,
    "from": 1,
    "last_page": 10,
    "per_page": 10,
    "to": 10,
    "total": 100
  }
}
```

## JSON Structure Explained

| Field       | Type    | Description |
| ----------- | ------- | ----------- |
| data[] | array | List of reviews. |
| data[].id | integer | Review ID. |
| data[].uuid | string | Review UUID. |
| data[].review | string | Review text content. |
| data[].rating | integer | Rating value (1-5). |
| data[].is_public | boolean | Whether review is public. |
| data[].created_at | string | Review creation date (ISO 8601). |
| data[].company | object | Company information. |
| data[].company.id | integer | Company ID. |
| data[].company.uuid | string | Company UUID. |
| data[].company.name | string | Company name. |
| data[].company.slug | string | Company slug. |
| data[].company.rating | object | Company rating information. |
| data[].company.rating.average | float | Average rating. |
| data[].company.rating.total | integer | Total number of ratings. |
| data[].company.logos[] | array | Company logos. |
| links | object | Pagination links. |
| meta | object | Pagination metadata. |

## HTTP Status

- 200: OK
- 400: Bad Request
- 401: Unauthorized (for `/my` route without authentication)
- 422: Unprocessable Entity
- 429: Too Many Requests
- 500: Internal Server Error

## Errors

Standard validation errors:

```json
{
  "message": "The given data was invalid.",
  "errors": {
    "rating": ["The rating must be between 1 and 5."]
  }
}
```

## Notes

- Unauthenticated requests automatically filter to public reviews only.
- The `/my` route returns only reviews from the authenticated user.
- Guest role users can only see their own reviews.
- Default pagination is 25 items per page.
- Reviews include related company data with logos and rating information.
- Use `keywords` parameter for full-text search in review content.

## Related

- [Create Company Review](CompanyReviewStore.md)
- [Show Company](CompanyShow.md)
- [List Companies Through Coverage](CompanyThroughCoverageIndex.md)
