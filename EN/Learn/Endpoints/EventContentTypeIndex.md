# Learn – List Event Content Types

## Endpoint

```
GET /api/v1/learn/events/content-types
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

No additional parameters required.

## Examples

### Request example (curl)

```bash
curl -X GET \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: en" \
  "https://sandbox.your-domain.com/api/v1/learn/events/content-types"
```

### Response example

```json
{
  "data": [
    {
      "id": "video",
      "name": "Video"
    },
    {
      "id": "audio",
      "name": "Audio"
    },
    {
      "id": "text",
      "name": "Text"
    },
    {
      "id": "pdf",
      "name": "PDF Document"
    },
    {
      "id": "quiz",
      "name": "Quiz"
    },
    {
      "id": "assignment",
      "name": "Assignment"
    }
  ]
}
```

## JSON Structure Explained

| Field       | Type   | Description |
| ----------- | ------ | ----------- |
| data[]      | array  | List of content types |
| data[].id   | string | Content type identifier |
| data[].name | string | Localized content type name |

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

- Returns all available event content types
- Content type names are localized based on the `Accept-Language` header
- The list is sorted alphabetically by name
- These types are used when creating or filtering event contents
- Common content types include: video, audio, text, PDF, quiz, assignment, interactive, live session

## Related

- [List Event Contents](./EventContentIndex.md)
- [Show Event Content](./EventContentShow.md)

## Changelog

- 2025-10-16: Initial documentation
