# Cms – Store Title

## Endpoint

```
POST /api/v1/cms/titles
```

## Authentication

Required – Bearer {token} with ability `store.platform.title`

## Headers

| Header           | Type   | Required | Description |
| ---------------- | ------ | -------- | ----------- |
| Authorization    | string | Yes      | `Bearer {token}`. |
| X-PUBLIC-KEY     | string | Yes      | Platform public key. |
| Accept-Language  | string | No       | IETF locale (e.g., `pt-BR`, `en`, `es`). |

## Parameters

### Path parameters

No path parameters.

### Request body

| Parameter       | Type    | Required | Description | Default/Values |
| --------------- | ------- | -------- | ----------- | -------------- |
| language        | string  | Yes      | Language code (ISO 639-1). | `en`, `es`, `pt`, `gn` |
| is_default      | boolean | Yes      | Whether this is the default translation. | |
| content         | string  | Yes      | Title content (max 255 characters). | |
| title           | string  | No       | Alias for `content` field. | |
| usage           | string  | No       | Usage context identifier (max 255 characters). | |
| additional_data | object  | No       | Additional data object. | |
| additional_data.subtitle | string | No | Subtitle text (max 255 characters). | |
| additional_data.text     | string | No | Extended text content (max 5000 characters). | |

> Parameter names accept `snake_case`, `camelCase`, `kebab-case`, or `CapitalCase` variants.
>
> **Note:** The `additional_data` field will be converted to `raw` internally by the request handler.

## Examples

### Request example (curl)

```bash
curl -X POST \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: en" \
  -H "Content-Type: application/json" \
  -d '{
    "language": "en",
    "is_default": true,
    "content": "Welcome to Our Platform",
    "usage": "homepage.hero",
    "additional_data": {
      "subtitle": "Your journey begins here",
      "text": "Discover amazing features and services designed to help you succeed."
    }
  }' \
  "https://sandbox.your-domain.com/api/v1/cms/titles"
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
    }
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

## HTTP Status

- 201: Created
- 400: Bad Request
- 401: Unauthorized
- 403: Forbidden
- 404: Not Found
- 422: Unprocessable Entity
- 429: Too Many Requests
- 500: Internal Server Error

## Errors

```json
{
  "message": "The given data was invalid.",
  "errors": {
    "language": ["The language field is required."],
    "is_default": ["The is default field is required."],
    "content": ["The content field is required."]
  }
}
```

## Notes

- The authenticated user must have the `store.platform.title` ability.
- Either `content` or `title` field can be used; they are aliases.
- The `additional_data` object is converted to the `raw` field internally.
- Maximum length for `content` is 255 characters.
- Maximum length for `usage` is 255 characters.
- Maximum length for `additional_data.subtitle` is 255 characters.
- Maximum length for `additional_data.text` is 5000 characters.
- All timestamps are in ISO 8601 format.

## Related

- [Cms Title Index](./CmsTitleIndex.md)
- [Cms Title Show](./CmsTitleShow.md)
- [Cms Domain](../README.md)

## Changelog

- 2025-10-23: Updated with complete request/response documentation.
- 2025-10-16: Initial documentation.
