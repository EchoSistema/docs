# Learn – Get Event Text

## Endpoint

```
GET /api/v1/learn/events/{event}/text/{usage}
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

### Path parameters

| Parameter | Type   | Required | Description |
| --------- | ------ | -------- | ----------- |
| event     | string | Yes      | Event text identifier (not resource name) |
| usage     | string | Yes      | Text usage type (e.g., description, about, terms) |

### Query parameters

| Parameter   | Type   | Required | Description | Default/Values |
| ----------- | ------ | -------- | ----------- | -------------- |
| platform_id | string | No       | Platform UUID for ownership validation | Derived from X-PUBLIC-KEY |

> The `event` parameter uses the `text` field binding. The endpoint accepts parameter name variations (`camelCase`, `kebab-case`, `CapitalCase`).

## Examples

### Request example (curl)

```bash
curl -X GET \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: en" \
  "https://sandbox.your-domain.com/api/v1/learn/events/event-text-123/text/description"
```

### Response example

```json
{
  "data": {
    "uuid": "bb0e8400-e29b-41d4-a716-446655440006",
    "usage": "description",
    "content": "This is a comprehensive course covering all aspects of web development...",
    "language": "en",
    "is_default": false,
    "created_at": "2025-01-15T10:30:00Z",
    "updated_at": "2025-10-10T14:20:00Z"
  }
}
```

## JSON Structure Explained

| Field              | Type    | Description |
| ------------------ | ------- | ----------- |
| data               | object  | Text content data |
| data.uuid          | string  | Text unique identifier |
| data.usage         | string  | Text usage type |
| data.content       | string  | Text content |
| data.language      | string  | Text language code |
| data.is_default    | boolean | Whether this is the default text |
| data.created_at    | string  | Creation timestamp (ISO 8601) |
| data.updated_at    | string  | Last update timestamp (ISO 8601) |

## HTTP Status

- 200: OK
- 400: Bad Request
- 401: Unauthorized
- 403: Forbidden - Platform does not own this event
- 404: Not Found - Event or text not found
- 429: Too Many Requests
- 500: Internal Server Error

## Errors

```json
{
  "message": "Text not found"
}
```

```json
{
  "message": "This event does not belong to the specified platform"
}
```

## Notes

- Returns a specific text content for an event based on usage type
- The event must belong to the platform making the request
- Platform ownership is validated using the `platform_id` parameter or derived from `X-PUBLIC-KEY`
- Common usage types include: `description`, `about`, `terms`, `prerequisites`, `objectives`
- Text content is returned based on the `Accept-Language` header when available

## Related

- [Show Event](./EventShow.md)
- [List Events](./EventIndex.md)

## Changelog

- 2025-10-16: Initial documentation
