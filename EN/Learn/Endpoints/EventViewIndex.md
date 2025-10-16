# Learn – List Event Views

## Endpoint

```
GET /api/v1/learn/v/events
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

| Parameter      | Type   | Required | Description | Default/Values |
| -------------- | ------ | -------- | ----------- | -------------- |
| platform_uuid  | string | No       | Platform UUID for filtering | Derived from X-PUBLIC-KEY |

> The endpoint accepts parameter name variations (`camelCase`, `kebab-case`, `CapitalCase`).

## Examples

### Request example (curl)

```bash
curl -X GET \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: en" \
  "https://sandbox.your-domain.com/api/v1/learn/v/events"
```

### Response example

```json
{
  "data": [
    {
      "_id": "507f1f77bcf86cd799439011",
      "event_uuid": "880e8400-e29b-41d4-a716-446655440003",
      "platform_uuid": "aa0e8400-e29b-41d4-a716-446655440005",
      "resource": "event-resource-name",
      "title": "Complete Web Development Course",
      "description": "Learn web development from scratch",
      "contents_count": 12,
      "total_duration_minutes": 540,
      "category": "Programming",
      "is_active": true,
      "created_at": "2025-01-15T10:30:00Z",
      "updated_at": "2025-10-10T14:20:00Z"
    },
    {
      "_id": "507f1f77bcf86cd799439012",
      "event_uuid": "990e8400-e29b-41d4-a716-446655440011",
      "platform_uuid": "aa0e8400-e29b-41d4-a716-446655440005",
      "resource": "another-event",
      "title": "Advanced JavaScript",
      "description": "Master modern JavaScript",
      "contents_count": 8,
      "total_duration_minutes": 360,
      "category": "Programming",
      "is_active": true,
      "created_at": "2025-02-20T08:00:00Z",
      "updated_at": "2025-09-15T16:45:00Z"
    }
  ]
}
```

## JSON Structure Explained

| Field                       | Type    | Description |
| --------------------------- | ------- | ----------- |
| data[]                      | array   | List of materialized event views |
| data[]._id                  | string  | MongoDB document identifier |
| data[].event_uuid           | string  | Event unique identifier |
| data[].platform_uuid        | string  | Associated platform UUID |
| data[].resource             | string  | Event resource name |
| data[].title                | string  | Event title |
| data[].description          | string  | Event description |
| data[].contents_count       | integer | Number of contents |
| data[].total_duration_minutes | integer | Total duration in minutes |
| data[].category             | string  | Event category |
| data[].is_active            | boolean | Whether the event is active |
| data[].created_at           | string  | Creation timestamp (ISO 8601) |
| data[].updated_at           | string  | Last update timestamp (ISO 8601) |

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

- Returns materialized views of events from MongoDB for optimized read performance
- Materialized views provide denormalized, pre-computed data for faster queries
- The data is read from a NoSQL database (MongoDB) separate from the main relational database
- Events are filtered by `platform_uuid` which is derived from the `X-PUBLIC-KEY` header
- This endpoint is optimized for listing and search operations
- The view may not reflect real-time changes; updates are synchronized asynchronously
- Useful for dashboards and reporting where read performance is critical

## Related

- [List Events](./EventIndex.md)
- [Show Event](./EventShow.md)

## Changelog

- 2025-10-16: Initial documentation
