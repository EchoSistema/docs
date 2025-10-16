# Learn – Show Event Content

## Endpoint

```
GET /api/v1/learn/events/{event}/contents/{index}
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

| Parameter | Type    | Required | Description |
| --------- | ------- | -------- | ----------- |
| event     | string  | Yes      | Event resource identifier |
| index     | integer | Yes      | Content index number |

### Query parameters

| Parameter | Type   | Required | Description | Default/Values |
| --------- | ------ | -------- | ----------- | -------------- |
| language  | string | No       | Language code for localized content | Platform default |

> The endpoint accepts parameter name variations (`camelCase`, `kebab-case`, `CapitalCase`).

## Examples

### Request example (curl)

```bash
curl -X GET \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: en" \
  "https://sandbox.your-domain.com/api/v1/learn/events/event-resource-name/contents/1?language=en"
```

### Response example

```json
{
  "data": {
    "uuid": "cc0e8400-e29b-41d4-a716-446655440007",
    "index_number": 1,
    "content_type": "video",
    "duration_minutes": 45,
    "is_preview": false,
    "event_title": {
      "language": "en",
      "value": "Complete Web Development Course"
    },
    "title": {
      "language": "en",
      "value": "Introduction to Web Development",
      "is_default": false
    },
    "description": {
      "language": "en",
      "value": "Learn the fundamentals of web development including HTML, CSS, and JavaScript basics."
    },
    "media": {
      "type": "video",
      "url": "https://example.com/video.mp4",
      "thumbnail": "https://example.com/thumbnail.jpg",
      "duration_seconds": 2700
    },
    "attachments": [
      {
        "name": "Course Material.pdf",
        "url": "https://example.com/material.pdf",
        "size_bytes": 1048576
      }
    ],
    "sub_contents_count": 3,
    "completed": false,
    "progress_percent": 0
  }
}
```

## JSON Structure Explained

| Field                     | Type    | Description |
| ------------------------- | ------- | ----------- |
| data                      | object  | Content details |
| data.uuid                 | string  | Content unique identifier |
| data.index_number         | integer | Content sequential index |
| data.content_type         | string  | Type of content |
| data.duration_minutes     | integer | Estimated duration in minutes |
| data.is_preview           | boolean | Whether this is a preview content |
| data.event_title          | object  | Parent event's localized title |
| data.title                | object  | Localized content title |
| data.description          | object  | Localized content description |
| data.media                | object  | Media information |
| data.attachments[]        | array   | Downloadable attachments |
| data.sub_contents_count   | integer | Number of sub-contents |
| data.completed            | boolean | User completion status (when authenticated) |
| data.progress_percent     | integer | User progress percentage (when authenticated) |

## HTTP Status

- 200: OK
- 400: Bad Request
- 401: Unauthorized
- 403: Forbidden
- 404: Not Found - Event or content not found
- 429: Too Many Requests
- 500: Internal Server Error

## Errors

```json
{
  "message": "Content not found"
}
```

## Notes

- Returns detailed information for a specific event content by its index number
- Content is identified by its `index_number` within the event
- Includes the parent event's title for context
- Content titles and descriptions are localized based on the `language` parameter or `Accept-Language` header
- If localized content is not available, default values are returned
- When the user is authenticated, includes completion status and progress information
- Preview contents are accessible without authentication

## Related

- [List Event Contents](./EventContentIndex.md)
- [List Event Sub-Contents](./EventContentSubContentsIndex.md)
- [Show Event](./EventShow.md)

## Changelog

- 2025-10-16: Initial documentation
