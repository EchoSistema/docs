# RealEstate – List Platform Texts

## Endpoint

```
GET /api/v1/real-estate/platform-texts
```

## Authentication

None

## Headers

| Header           | Type   | Required | Description |
| ---------------- | ------ | -------- | ----------- |
| X-PUBLIC-KEY     | string | Yes      | Platform public key. |
| Accept-Language  | string | No       | IETF locale (e.g., `pt-BR`, `en`, `es`). |

## Parameters

No query parameters required.

## Examples

### Request example (curl)

```bash
curl -X GET \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: en" \
  "https://sandbox.your-domain.com/api/v1/real-estate/platform-texts"
```

### Response example

```json
{
  "data": [
    {
      "uuid": "cc0e8400-e29b-41d4-a716-446655440001",
      "key": "welcome_message",
      "content": "Find your dream home with us",
      "usage": "homepage",
      "language": "en"
    },
    {
      "uuid": "cc0e8400-e29b-41d4-a716-446655440002",
      "key": "about_us",
      "content": "We are a leading real estate platform...",
      "usage": "about",
      "language": "en"
    }
  ],
  "meta": {
    "total": 2
  }
}
```

## JSON Structure Explained

| Field       | Type    | Description |
| ----------- | ------- | ----------- |
| data[] | array | List of platform texts |
| data[].uuid | string | Text unique identifier |
| data[].key | string | Text key identifier for referencing |
| data[].content | string | Text content |
| data[].usage | string | Text usage context (homepage, about, footer, etc.) |
| data[].language | string | Language code of the text |
| meta | object | Response metadata |
| meta.total | integer | Total number of texts |

## HTTP Status

- 200: OK
- 429: Too Many Requests
- 500: Internal Server Error

## Errors

```json
{
  "message": "Error message"
}
```

## Notes

- Returns all platform-specific texts for customization
- Texts are filtered by the `X-PUBLIC-KEY` header
- Content respects the `Accept-Language` header for translations
- Use the `key` field to identify specific texts in your application
- Results are not paginated as the list is typically small

## Related

- [List Platform Images](PlatformImageIndex.md)

## Changelog

- 2025-10-16: Initial documentation
