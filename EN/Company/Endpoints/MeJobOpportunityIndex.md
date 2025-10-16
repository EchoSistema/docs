# Company – List Job Opportunities

## Endpoint

```
GET /api/v1/company/opportunities
```

## Authentication

None

## Headers

| Header           | Type   | Required | Description |
| ---------------- | ------ | -------- | ----------- |
| X-PUBLIC-KEY     | string | Yes      | Platform public key. |
| Accept-Language  | string | No       | IETF locale (e.g., `pt-BR`, `en`, `es`). |

## Parameters

### Query parameters

| Parameter   | Type    | Required | Description | Default/Values |
| ----------- | ------- | -------- | ----------- | -------------- |
| language    | string  | No       | Language code for translatable fields (e.g., `en`, `pt-BR`, `es`). | Platform default |
| noPaginate  | boolean | No       | Disable pagination when `true`. | `false` |
| perPage     | integer | No       | Number of items per page when paginated. | 25 |
| page        | integer | No       | Page number when paginated. | 1 |
| withTrash   | boolean | No       | Include soft-deleted opportunities. | `false` |

> Parameter names accept `snake_case`, `camelCase`, `kebab-case`, or `CapitalCase` variants.

## Examples

### Request example (curl)

```bash
curl -X GET \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: en" \
  "https://sandbox.your-domain.com/api/v1/company/opportunities?language=en&perPage=10"
```

### Response example (paginated)

```json
{
  "data": [
    {
      "uuid": "00000000-0000-0000-0000-000000000001",
      "company": "Tech Solutions Inc.",
      "total_job_openings": 3,
      "title": "Senior Software Developer",
      "mode": "remote",
      "starts_at": "2025-10-20T00:00:00+00:00",
      "finishes_at": "2025-11-30T23:59:59+00:00"
    },
    {
      "uuid": "00000000-0000-0000-0000-000000000002",
      "company": "Tech Solutions Inc.",
      "total_job_openings": 2,
      "title": "DevOps Engineer",
      "mode": "hybrid",
      "starts_at": "2025-10-15T00:00:00+00:00",
      "finishes_at": "2025-12-15T23:59:59+00:00"
    }
  ],
  "links": {
    "first": "https://sandbox.your-domain.com/api/v1/company/opportunities?page=1",
    "last": "https://sandbox.your-domain.com/api/v1/company/opportunities?page=5",
    "prev": null,
    "next": "https://sandbox.your-domain.com/api/v1/company/opportunities?page=2"
  },
  "meta": {
    "current_page": 1,
    "from": 1,
    "last_page": 5,
    "path": "https://sandbox.your-domain.com/api/v1/company/opportunities",
    "per_page": 10,
    "to": 10,
    "total": 47
  }
}
```

## JSON Structure Explained

| Field                    | Type     | Description |
| ------------------------ | -------- | ----------- |
| data[]                   | array    | List of job opportunities. |
| data[].uuid              | string   | Job opportunity UUID. |
| data[].company           | string   | Company name (only when not owner). |
| data[].total_job_openings| integer  | Number of open positions. |
| data[].title             | string   | Job title in requested language. |
| data[].mode              | string   | Work mode: `remote`, `hybrid`, `on-site`. |
| data[].starts_at         | datetime | Opportunity start date. |
| data[].finishes_at       | datetime | Opportunity end date. |
| links                    | object   | Pagination links (when paginated). |
| links.first              | string   | URL to first page. |
| links.last               | string   | URL to last page. |
| links.prev               | string   | URL to previous page. |
| links.next               | string   | URL to next page. |
| meta                     | object   | Pagination metadata (when paginated). |
| meta.current_page        | integer  | Current page number. |
| meta.from                | integer  | Starting item index. |
| meta.last_page           | integer  | Total number of pages. |
| meta.path                | string   | Base URL path. |
| meta.per_page            | integer  | Items per page. |
| meta.to                  | integer  | Ending item index. |
| meta.total               | integer  | Total number of items. |

## HTTP Status

- 200: OK
- 400: Bad Request
- 401: Unauthorized
- 404: Not Found
- 429: Too Many Requests
- 500: Internal Server Error

## Errors

```json
{
  "message": "Invalid platform key.",
  "errors": {}
}
```

## Notes

- Returns job opportunities for the company associated with the current platform (identified by `X-PUBLIC-KEY`).
- Opportunities are ordered by `finishes_at` in descending order (newest first).
- When `noPaginate=true`, pagination metadata (`links` and `meta`) is omitted.
- The `company` field is only included in the response when the request is not from the owner.
- Soft-deleted opportunities are excluded by default. Use `withTrash=true` to include them.
- Job titles and descriptions are returned in the requested language, falling back to the default language if unavailable.

## Related

- [Job Opportunity Show](./MeJobOpportunityShow.md)
- [Company Show](./CompanyShow.md)

## Changelog

- 2025-10-16: Initial documentation.
