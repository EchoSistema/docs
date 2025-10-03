# Shared – Available Languages Index

## Endpoint

`GET /api/v1/languages`

Returns the backoffice languages declared in `LanguageEnum`, including the code and presented name (`native_name`).

---

## Authentication

None.

---

## Request

No parameters.

---

## Example Response

```json
{
  "data": [
    {
      "name": "PT_BR",
      "code": "pt-BR",
      "native_name": "Portugues (Brasil)"
    },
    {
      "name": "EN",
      "code": "en",
      "native_name": "English"
    }
  ]
}
```

---

## JSON Structure Explanation

| Field | Type | Description |
| ----- | ---- | ----------- |
| `data[]` | `array` | Supported languages list. |
| `data[].name` | `string` | Enum name (`PT_BR`, `EN`, `ES`, `GN`). |
| `data[].code` | `string` | ISO code for the language. |
| `data[].native_name` | `string` | Display name for the language. |

---

## Notes

* Ordering follows the declaration in `LanguageEnum`.

---

## Changelog

- 2025-10-03: Initial documentation for `/api/v1/languages`.
