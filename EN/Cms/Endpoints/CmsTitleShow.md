# Cms – Show Title

## Endpoint

```
GET /api/v1/cms/titles/{title}
```

## Authentication

None

## Headers

| Header           | Type   | Required | Description |
| ---------------- | ------ | -------- | ----------- |
| X-PUBLIC-KEY     | string | Yes      | Platform public key. |
| Accept-Language  | string | No       | IETF locale (e.g., `pt-BR`, `en`, `es`). |

## Parameters

### Path parameters

| Parameter | Type   | Required | Description |
| --------- | ------ | -------- | ----------- |
| title     | string | Yes      | Title UUID identifier. |

### Query parameters

| Parameter | Type   | Required | Description | Default/Values |
| --------- | ------ | -------- | ----------- | -------------- |
| language  | string | No       | Filter relations by language code. | |

> Parameter names accept `snake_case`, `camelCase`, `kebab-case`, or `CapitalCase` variants.

## Examples

### Request example (curl)

```bash
curl -X GET \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: en" \
  "https://sandbox.your-domain.com/api/v1/cms/titles/9d4e8f2a-1234-5678-90ab-cdef12345678?language=en"
```

### Response example

```json
{
  "data": {
    "uuid": "9d4e8f2a-1234-5678-90ab-cdef12345678",
    "language": "en",
    "is_default": true,
    "content": "Welcome to Our Platform",
    "usage": "homepage.hero",
    "raw": {
      "subtitle": "Your journey begins here",
      "text": "Discover amazing features and services designed to help you succeed."
    },
    "created_at": "2025-10-23T12:00:00Z",
    "updated_at": "2025-10-23T12:00:00Z",
    "text": {
      "uuid": "8c3d7e1b-5678-90ab-cdef-123456789012",
      "language": "en",
      "is_default": true,
      "content": "Welcome to Our Platform"
    },
    "titles": [
      {
        "uuid": "7b2c6d0a-4567-89ab-cdef-0123456789ab",
        "language": "en",
        "is_default": true,
        "content": "Platform Overview"
      }
    ]
  }
}
```

## JSON Structure Explained

| Field              | Type     | Description |
| ------------------ | -------- | ----------- |
| data               | object   | Title resource. |
| data.uuid          | string   | Title UUID identifier. |
| data.language      | string   | Language code (ISO 639-1). |
| data.is_default    | boolean  | Whether this is the default translation. |
| data.content       | string   | Title content. |
| data.usage         | string   | Usage context identifier. |
| data.raw           | object   | Additional data stored as raw JSON. |
| data.raw.subtitle  | string   | Subtitle text. |
| data.raw.text      | string   | Extended text content. |
| data.created_at    | datetime | Creation timestamp in ISO 8601 format. |
| data.updated_at    | datetime | Last update timestamp in ISO 8601 format. |
| data.text          | object   | Related text resource. |
| data.text.uuid     | string   | Text UUID identifier. |
| data.text.language | string   | Text language code. |
| data.text.is_default | boolean | Whether text is default translation. |
| data.text.content  | string   | Text content. |
| data.titles        | array    | Related titles collection. |
| data.titles[].uuid | string   | Related title UUID identifier. |
| data.titles[].language | string | Related title language code. |
| data.titles[].is_default | boolean | Whether related title is default translation. |
| data.titles[].content | string | Related title content. |

## HTTP Status

- 200: OK
- 400: Bad Request
- 401: Unauthorized
- 403: Forbidden
- 404: Not Found
- 422: Unprocessable Entity
- 429: Too Many Requests
- 500: Internal Server Error

## Notes

- The `title` path parameter must be a valid UUID.
- When `language` is provided, related `text` and `titles` will be loaded for that language or default translations.
- All timestamps are in ISO 8601 format.

## Related

- [Cms Title Index](./CmsTitleIndex.md)
- [Cms Title Store](./CmsTitleStore.md)
- [Cms Domain](../README.md)

## Changelog

- 2025-10-23: Updated with complete response documentation and query parameters.
- 2025-10-16: Initial documentation.
