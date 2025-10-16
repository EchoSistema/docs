# Learn – List Event Contents

## Endpoint

```
GET /api/v1/learn/events/{event}/contents
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
| event     | string | Yes      | Event resource identifier |

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
  "https://sandbox.your-domain.com/api/v1/learn/events/event-resource-name/contents?language=en"
```

### Response example

```json
{
  "data": [
    {
      "uuid": "cc0e8400-e29b-41d4-a716-446655440007",
      "index_number": 1,
      "content_type": "video",
      "duration_minutes": 45,
      "is_preview": false,
      "title": {
        "language": "en",
        "value": "Introduction to Web Development",
        "is_default": false
      },
      "description": {
        "language": "en",
        "value": "Learn the fundamentals of web development..."
      },
      "media": {
        "url": "https://example.com/video.mp4",
        "thumbnail": "https://example.com/thumbnail.jpg"
      },
      "sub_contents_count": 3
    },
    {
      "uuid": "dd0e8400-e29b-41d4-a716-446655440008",
      "index_number": 2,
      "content_type": "text",
      "duration_minutes": 15,
      "is_preview": true,
      "title": {
        "language": "en",
        "value": "HTML Basics"
      },
      "sub_contents_count": 0
    }
  ]
}
```

## JSON Structure Explained

| Field                      | Type    | Description |
| -------------------------- | ------- | ----------- |
| data[]                     | array   | List of event contents |
| data[].uuid                | string  | Content unique identifier |
| data[].index_number        | integer | Content sequential index |
| data[].content_type        | string  | Type of content (video, text, pdf, etc.) |
| data[].duration_minutes    | integer | Estimated duration in minutes |
| data[].is_preview          | boolean | Whether this content is available as preview |
| data[].title               | object  | Localized content title |
| data[].description         | object  | Localized content description |
| data[].media               | object  | Media URLs (when applicable) |
| data[].sub_contents_count  | integer | Number of sub-contents |

## HTTP Status

- 200: OK
- 400: Bad Request
- 401: Unauthorized
- 403: Forbidden
- 404: Not Found - Event not found
- 429: Too Many Requests
- 500: Internal Server Error

## Errors

```json
{
  "message": "Event not found"
}
```

## Notes

- Returns all contents for a specific event
- Contents are ordered by `index_number`
- Content titles and descriptions are localized based on the `language` parameter or `Accept-Language` header
- If localized content is not available, default values are returned
- The `sub_contents_count` field indicates if the content has nested sub-contents
- Preview contents (`is_preview: true`) are typically available without authentication

## Related

- [Show Event Content](./EventContentShow.md)
- [List Event Sub-Contents](./EventContentSubContentsIndex.md)
- [Show Event](./EventShow.md)

## Changelog

- 2025-10-16: Initial documentation
