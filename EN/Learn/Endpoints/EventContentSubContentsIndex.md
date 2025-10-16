# Learn – List Event Sub-Contents

## Endpoint

```
GET /api/v1/learn/events/{event}/contents/{index}/sub-contents
```

## Authentication

Required – Bearer {token}

## Headers

| Header          | Type   | Required | Description |
| --------------- | ------ | -------- | ----------- |
| Authorization   | string | Yes      | `Bearer {token}`. |
| X-PUBLIC-KEY    | string | Yes      | Platform public key. |
| Accept-Language | string | No       | IETF locale (e.g., `pt-BR`, `en`, `es`). |

## Parameters

### Path parameters

| Parameter | Type    | Required | Description |
| --------- | ------- | -------- | ----------- |
| event     | string  | Yes      | Event resource identifier |
| index     | integer | Yes      | Parent content index number |

### Query parameters

| Parameter   | Type   | Required | Description | Default/Values |
| ----------- | ------ | -------- | ----------- | -------------- |
| language    | string | No       | Language code for localized content | Platform default |
| platform_id | string | No       | Platform UUID for ownership validation | Derived from X-PUBLIC-KEY |

> The endpoint accepts parameter name variations (`camelCase`, `kebab-case`, `CapitalCase`).

## Examples

### Request example (curl)

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: en" \
  "https://sandbox.your-domain.com/api/v1/learn/events/event-resource-name/contents/1/sub-contents?language=en"
```

### Response example

```json
{
  "data": [
    {
      "uuid": "ee0e8400-e29b-41d4-a716-446655440009",
      "index_number": 1,
      "parent_content_index": 1,
      "content_type": "video",
      "duration_minutes": 15,
      "title": {
        "language": "en",
        "value": "HTML Basics - Part 1",
        "is_default": false
      },
      "description": {
        "language": "en",
        "value": "Introduction to HTML tags and structure"
      },
      "media": {
        "url": "https://example.com/sub-video-1.mp4",
        "thumbnail": "https://example.com/sub-thumb-1.jpg"
      }
    },
    {
      "uuid": "ff0e8400-e29b-41d4-a716-446655440010",
      "index_number": 2,
      "parent_content_index": 1,
      "content_type": "quiz",
      "duration_minutes": 5,
      "title": {
        "language": "en",
        "value": "HTML Knowledge Check"
      }
    }
  ]
}
```

## JSON Structure Explained

| Field                        | Type    | Description |
| ---------------------------- | ------- | ----------- |
| data[]                       | array   | List of sub-contents |
| data[].uuid                  | string  | Sub-content unique identifier |
| data[].index_number          | integer | Sub-content sequential index |
| data[].parent_content_index  | integer | Parent content index number |
| data[].content_type          | string  | Type of sub-content |
| data[].duration_minutes      | integer | Estimated duration in minutes |
| data[].title                 | object  | Localized sub-content title |
| data[].description           | object  | Localized sub-content description |
| data[].media                 | object  | Media URLs (when applicable) |

## HTTP Status

- 200: OK
- 400: Bad Request
- 401: Unauthorized - Authentication required
- 403: Forbidden - Platform does not own this event
- 404: Not Found - Event or content not found
- 429: Too Many Requests
- 500: Internal Server Error

## Errors

```json
{
  "message": "Content not found"
}
```

```json
{
  "message": "Unauthenticated"
}
```

## Notes

- Returns all sub-contents for a specific parent content
- Requires authentication as sub-contents are typically not preview content
- The event must belong to the platform making the request
- Platform ownership is validated using the `platform_id` parameter or derived from `X-PUBLIC-KEY`
- Sub-contents are ordered by `index_number`
- Titles and descriptions are localized based on the `language` parameter or `Accept-Language` header
- If localized content is not available, default values are returned

## Related

- [Show Event Sub-Content](./EventContentSubContentShow.md)
- [Show Event Content](./EventContentShow.md)
- [List Event Contents](./EventContentIndex.md)

## Changelog

- 2025-10-16: Initial documentation
