# Shared – Authenticated user profile

## Endpoint

```
GET /api/v1/me/profile
```

Returns profile details for the authenticated user, including personal data, address, preferred language/currency, and linked nationalities.

## Authentication

Required – Bearer {token} (Laravel Sanctum).

## Headers

| Header         | Type   | Required | Description |
| -------------- | ------ | -------- | ----------- |
| Authorization  | string | Yes      | `Bearer {token}` for the current user session. |
| X-PUBLIC-KEY   | string | Yes      | Platform public key header. |
| Accept-Language| string | No       | IETF locale (e.g., `pt-BR`, `en`, `es`). |

## Parameters

No path, query, or body parameters are required.

## Examples

### Request (curl)

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: en" \
  "https://sandbox.echosistema.com/api/v1/me/profile"
```

### Success response

```json
{
  "data": {
    "uuid": "86b77a09-5f27-4c37-bb8f-5d0ba1e93f9d",
    "name": "Maria Silva",
    "email": "maria.silva@example.com",
    "avatar": "https://cdn.echosistema.com/avatars/maria.webp",
    "roles": [
      {
        "id": 2,
        "uuid": "1bdf9f1d-22c4-4248-8a7a-5b19b0e45f26",
        "name": "ADMIN",
        "localized_name": "Administrator",
        "permissions": ["platform.manage", "user.read"]
      }
    ],
    "telephone": "+55 11 91234-5678",
    "language": "pt-BR",
    "currency": "BRL",
    "gender": "female",
    "birthday": "1990-01-20T00:00:00+00:00",
    "address": {
      "uuid": "5f2e4f7b-6d9a-4f8a-9648-2f6f79d2cb6e",
      "postal_code": "01310-000",
      "street": "Av. Paulista",
      "number": "1000",
      "district": "Bela Vista",
      "city": "São Paulo",
      "state": "SP",
      "country": "Brazil"
    },
    "nationalities": [
      {
        "uuid": "d5e1f2e9-27d5-4bd8-90c2-1887a57c6510",
        "country": {
          "name": "Brazil",
          "code": "BR",
          "flag": "🇧🇷"
        }
      }
    ]
  }
}
```

## JSON Structure Explained

| Field                | Type   | Description |
| -------------------- | ------ | ----------- |
| data.uuid            | uuid   | User identifier. |
| data.name            | string | Full name. |
| data.email           | string | Registered email address. |
| data.avatar          | string | Avatar URL. Can be `null` when absent. |
| data.roles[]         | array  | Present when the `platformRoles` relation is loaded. Each entry exposes `id`, `uuid`, `name`, `localized_name`, and `permissions`. |
| data.telephone       | string | Primary phone number when available. |
| data.language        | string | Preferred language derived from `LanguageEnum`. |
| data.currency        | string | Preferred currency derived from `CurrencyEnum`. |
| data.gender          | string | Stored gender. |
| data.birthday        | string | Birth date in ISO-8601 (`YYYY-MM-DDTHH:MM:SSZ`). Can be `null`. |
| data.address         | object | Primary address when the relation is loaded. Uses `AddressResource`. |
| data.nationalities[] | array  | Nationalities linked to the user, each with `uuid` and `country` (`name`, `code`, `flag`). |

## HTTP Status

- 200: Profile returned successfully.
- 401: Unauthenticated (missing or invalid token).
- 500: Unexpected server error.

## Errors

```json
{
  "message": "Unauthenticated."
}
```

## Notes

- Route name: `api.v1.profile`.
- The controller eagerly loads `address`, `avatarImage`, and `telephone`. Other relations (e.g., `platformRoles`, `nationalities`) appear only when previously eager-loaded.
- The avatar is resolved by `UserProfileResource::getAvatar()` and may depend on storage integrations (S3).

## Related

- [Shared – Update user language](UserLanguageUpdate.md)
- [Shared – Update user currency](UserCurrencyUpdate.md)

## Changelog

- 2025-10-14: Endpoint documented.
