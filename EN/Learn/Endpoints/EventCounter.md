# Learn – Get Event Counter

## Endpoint

```
GET /api/v1/learn/events/counter
```

## Authentication

None

## Headers

| Header          | Type   | Required | Description |
| --------------- | ------ | -------- | ----------- |
| Authorization   | string | No       | `Bearer {token}` (optional). |
| X-PUBLIC-KEY    | string | Yes      | Platform public key. |
| Accept-Language | string | No       | IETF locale (e.g., `pt-BR`, `en`, `es`). |

## Parameters

### Query parameters

Additional filter parameters are available through EventCounterFilter.

> The endpoint accepts parameter name variations (`camelCase`, `kebab-case`, `CapitalCase`).

## Examples

### Request example (curl)

```bash
curl -X GET \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: en" \
  "https://sandbox.your-domain.com/api/v1/learn/events/counter"
```

### Response example

```json
{
  "data": {
    "total": 47,
    "active": 42,
    "inactive": 5,
    "by_category": {
      "programming": 25,
      "design": 12,
      "business": 10
    }
  }
}
```

## JSON Structure Explained

| Field                  | Type    | Description |
| ---------------------- | ------- | ----------- |
| data                   | object  | Counter data |
| data.total             | integer | Total number of events |
| data.active            | integer | Number of active events |
| data.inactive          | integer | Number of inactive events |
| data.by_category       | object  | Event count grouped by category |

## HTTP Status

- 200: OK
- 400: Bad Request
- 401: Unauthorized
- 403: Forbidden
- 404: Not Found
- 429: Too Many Requests
- 500: Internal Server Error

## Errors

```json
{
  "message": "Invalid platform key"
}
```

## Notes

- Returns aggregated event counts for the platform
- Supports filtering through EventCounterFilter
- Useful for dashboard statistics and analytics
- The counter respects platform ownership and only counts events belonging to the authenticated platform

## Related

- [List Events](./EventIndex.md)
- [Show Event](./EventShow.md)

## Changelog

- 2025-10-16: Initial documentation
