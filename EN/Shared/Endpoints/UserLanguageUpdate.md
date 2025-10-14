# Shared – Update user language

## Endpoint

```
PATCH /api/v1/user/language
```

Updates the preferred language for the authenticated user and persists the value in the regional information table.

## Authentication

Required – Bearer {token} (Laravel Sanctum).

## Headers

| Header         | Type   | Required | Description |
| -------------- | ------ | -------- | ----------- |
| Authorization  | string | Yes      | `Bearer {token}` belonging to the authenticated user. |
| X-PUBLIC-KEY   | string | Yes      | Platform public key header. |
| Accept-Language| string | No       | IETF locale (e.g., `pt-BR`, `en`, `es`). |

## Request body

```json
{
  "language": "pt-BR"
}
```

| Field    | Type   | Required | Description |
| -------- | ------ | -------- | ----------- |
| language | string | Yes      | Enum value from `LanguageEnum` (`pt-BR`, `en`, `es`, `gn`). |

## Examples

### Request (curl)

```bash
curl -X PATCH \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: en" \
  -H "Content-Type: application/json" \
  -d '{"language":"pt-BR"}' \
  "https://sandbox.echosistema.com/api/v1/user/language"
```

### Success response

```json
{
  "data": {
    "language": "pt-BR"
  },
  "meta": []
}
```

## JSON Structure Explained

| Field         | Type   | Description |
| ------------- | ------ | ----------- |
| data.language | string | Language saved for the user. |
| meta          | array  | Reserved for metadata (empty array). |

## HTTP Status

- 200: Language updated successfully.
- 401: Missing or invalid token.
- 422: `language` not allowed by the enum.
- 500: Unexpected server error.

## Errors

```json
{
  "message": "The selected language is invalid.",
  "errors": {
    "language": [
      "The language field is invalid."
    ]
  }
}
```

## Notes

- Route name: `api.v1.user.language.update`.
- The value is stored in `regional_information.language` using `LanguageEnum`.
- If the user has no regional information record, it is created automatically.

## Related

- [Shared – Update user currency](UserCurrencyUpdate.md)
- [Shared – Available languages](LanguageIndex.md)

## Changelog

- 2025-10-14: Endpoint documented.
