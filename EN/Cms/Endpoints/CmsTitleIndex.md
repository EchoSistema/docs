# Cms – List Titles

## Endpoint

```
GET /api/v1/cms/titles
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

No path parameters.

### Query parameters

| Parameter      | Type    | Required | Description | Default/Values |
| -------------- | ------- | -------- | ----------- | -------------- |
| language       | string  | No       | Filter by language code. | |
| uuid           | string  | No       | Filter by UUID. | |
| all_language   | boolean | No       | Include all languages. | `false` |
| is_default     | boolean | No       | Filter by default translation. | |
| usage          | string  | No       | Filter by exact usage context. | |
| usage_like     | string  | No       | Filter by usage context (partial match). | |
| with_text      | boolean | No       | Include text relation. | `false` |
| with_texts     | boolean | No       | Include texts relation. | `false` |
| per_page       | integer | No       | Results per page. | `25` (1-100) |
| page           | integer | No       | Page number. | `1` |
| no_paginate    | boolean | No       | Disable pagination. | `false` |
| sort           | string  | No       | Sort field. | `content` |
| direction      | string  | No       | Sort direction. | `asc` (`asc`, `desc`) |

> Parameter names accept `snake_case`, `camelCase`, `kebab-case`, or `CapitalCase` variants.

## Examples

### Request example (curl)

```bash
curl -X GET \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: en" \
  "https://sandbox.your-domain.com/api/v1/cms/titles?language=en&usage=homepage.hero&per_page=10&page=1"
```

### Response example

```json
{
  "data": [
    {
      "uuid": "9d4e8f2a-1234-5678-90ab-cdef12345678",
      "language": "en",
      "is_default": true,
      "content": "Welcome to Our Platform",
      "usage": "homepage.hero",
      "created_at": "2025-10-23T12:00:00Z",
      "updated_at": "2025-10-23T12:00:00Z",
      "text": {
        "uuid": "8c3d7e1b-5678-90ab-cdef-123456789012",
        "language": "en",
        "is_default": true,
        "content": "Welcome to Our Platform"
      }
    }
  ],
  "links": {
    "first": "https://sandbox.your-domain.com/api/v1/cms/titles?page=1",
    "last": "https://sandbox.your-domain.com/api/v1/cms/titles?page=5",
    "prev": null,
    "next": "https://sandbox.your-domain.com/api/v1/cms/titles?page=2"
  },
  "meta": {
    "current_page": 1,
    "from": 1,
    "last_page": 5,
    "path": "https://sandbox.your-domain.com/api/v1/cms/titles",
    "per_page": 10,
    "to": 10,
    "total": 50
  }
}
```

## JSON Structure Explained

| Field              | Type     | Description |
| ------------------ | -------- | ----------- |
| data               | array    | Collection of title resources. |
| data[].uuid        | string   | Title UUID identifier. |
| data[].language    | string   | Language code (ISO 639-1). |
| data[].is_default  | boolean  | Whether this is the default translation. |
| data[].content     | string   | Title content. |
| data[].usage       | string   | Usage context identifier. |
| data[].created_at  | datetime | Creation timestamp in ISO 8601 format. |
| data[].updated_at  | datetime | Last update timestamp in ISO 8601 format. |
| data[].text        | object   | Related text resource (when requested). |
| data[].text.uuid   | string   | Text UUID identifier. |
| data[].text.language | string | Text language code. |
| data[].text.is_default | boolean | Whether text is default translation. |
| data[].text.content | string  | Text content. |
| data[].titles      | array    | Related titles collection (when requested). |
| links              | object   | Pagination links. |
| meta               | object   | Pagination metadata. |

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

- The `language` parameter filters titles by the specified language code.
- When `language` is provided, related `text` and `titles` will be loaded for that language or default translations.
- The `usage_like` parameter allows partial matching on the usage field.
- The `no_paginate` parameter returns all results without pagination.
- Default sort is by `content` in ascending order.
- All timestamps are in ISO 8601 format.

## Related

- [Cms Title Store](./CmsTitleStore.md)
- [Cms Title Show](./CmsTitleShow.md)
- [Cms Domain](../README.md)

## Changelog

- 2025-10-23: Updated with complete query parameters and response documentation.
- 2025-10-16: Initial documentation.
