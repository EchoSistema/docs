# RealEstate – List Platform Images

## Endpoint

```
GET /api/v1/real-estate/platform-images
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
  "https://sandbox.your-domain.com/api/v1/real-estate/platform-images"
```

### Response example

```json
{
  "data": [
    {
      "uuid": "bb0e8400-e29b-41d4-a716-446655440001",
      "key": "hero_banner",
      "url": "https://example.com/images/hero-banner.jpg",
      "alt": "Real Estate Platform Hero Banner",
      "usage": "homepage",
      "width": 1920,
      "height": 1080
    },
    {
      "uuid": "bb0e8400-e29b-41d4-a716-446655440002",
      "key": "logo",
      "url": "https://example.com/images/logo.png",
      "alt": "Platform Logo",
      "usage": "branding",
      "width": 200,
      "height": 80
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
| data[] | array | List of platform images |
| data[].uuid | string | Image unique identifier |
| data[].key | string | Image key identifier for referencing |
| data[].url | string | Image URL |
| data[].alt | string | Alternative text for accessibility |
| data[].usage | string | Image usage context (homepage, branding, etc.) |
| data[].width | integer | Image width in pixels |
| data[].height | integer | Image height in pixels |
| meta | object | Response metadata |
| meta.total | integer | Total number of images |

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

- Returns all platform-specific images for customization
- Images are filtered by the `X-PUBLIC-KEY` header
- Use the `key` field to identify specific images in your application
- Results are not paginated as the list is typically small
- Images are optimized and served through CDN

## Related

- [List Platform Texts](PlatformTextIndex.md)

## Changelog

- 2025-10-16: Initial documentation
