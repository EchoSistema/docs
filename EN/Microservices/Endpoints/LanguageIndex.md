# Microservices – Languages Index

## Endpoint

```
GET /api/v1/public/languages
```

Returns the most widely known languages worldwide based on `public/storage/documents/languages.json`. Useful for public frontends that need to populate language pickers.

---

## Authentication

None.

---

## Headers

| Header          | Type   | Required | Description |
| --------------- | ------ | -------- | ----------- |
| Accept-Language | string | No       | IETF locale (`pt-BR`, `en`, `es`). |

---

## Parameters

None.

---

## Examples

### Request (curl)

```bash
curl -X GET "https://sandbox.example.com/api/v1/public/languages"
```

### Response

```json
{
  "data": [
    {"name": "English", "code": "en"},
    {"name": "Spanish", "code": "es"},
    {"name": "Portuguese", "code": "pt"}
  ]
}
```

---

## JSON Structure Explanation

| Field         | Type   | Description |
| ------------- | ------ | ----------- |
| `data[]`      | array  | Supported language list. |
| `data[].name` | string | Human readable language name. |
| `data[].code` | string | ISO-639-1 code. |

---

## Notes

- The endpoint lists globally known languages consumed by partner apps.
- Add or fix entries by editing `public/storage/documents/languages.json`.
- `LanguageRepository` parses the JSON and gracefully handles read errors.

---

## Changelog

- 2025-10-03: Initial documentation for the public languages endpoint.
