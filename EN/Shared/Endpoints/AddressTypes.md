# Shared – Get Address Types

## Endpoint

```
GET /api/v1/addresses/types
```

## Authentication

Required – Bearer {token} with ability `backoffice`

## Headers

| Header           | Type   | Required | Description |
| ---------------- | ------ | -------- | ----------- |
| Authorization    | string | Yes      | `Bearer {token}`. |
| X-PUBLIC-KEY     | string | Yes      | Platform public key. |
| Accept-Language  | string | No       | IETF locale (e.g., `pt-BR`, `en`, `es`). Affects the `title_localized` field translation. |

## Parameters

This endpoint does not accept any parameters.

## Examples

### Request example (curl)

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: en" \
  "https://sandbox.your-domain.com/api/v1/addresses/types"
```

### Request example (JavaScript)

```javascript
const response = await fetch('https://sandbox.your-domain.com/api/v1/addresses/types', {
  method: 'GET',
  headers: {
    'Authorization': 'Bearer <token>',
    'X-PUBLIC-KEY': '<key>',
    'Accept-Language': 'en',
  }
});
```

### Response example

```json
{
  "data": [
    {
      "name": "BILLING",
      "value": "billing",
      "title_localized": "Billing"
    },
    {
      "name": "SHIPPING",
      "value": "shipping",
      "title_localized": "Shipping"
    },
    {
      "name": "BOTH",
      "value": "both",
      "title_localized": "Both"
    },
    {
      "name": "FISCAL",
      "value": "fiscal",
      "title_localized": "Fiscal"
    },
    {
      "name": "ADDITIONAL",
      "value": "additional",
      "title_localized": "Additional"
    }
  ]
}
```

## JSON Structure Explained

| Field                  | Type   | Description |
| ---------------------- | ------ | ----------- |
| data                   | array  | List of available address types. |
| data[].name            | string | Enum constant name (uppercase). |
| data[].value           | string | Enum value (lowercase). Use this value when creating/updating addresses. |
| data[].title_localized | string | Human-readable translated title based on `Accept-Language` header. |

## HTTP Status

- 200: OK
- 401: Unauthorized
- 403: Forbidden
- 429: Too Many Requests
- 500: Internal Server Error

## Errors

### Unauthenticated

```json
{
  "message": "Unauthenticated.",
  "errors": {}
}
```

### Forbidden

```json
{
  "message": "This action is unauthorized.",
  "errors": {}
}
```

## Notes

- This endpoint returns all available address types from the `AddressTypeEnum`.
- The `title_localized` field is automatically translated based on the `Accept-Language` header.
- Use the `value` field when creating or updating addresses (e.g., `"type": "billing"`).
- Supported languages: `en` (English), `pt-BR` (Brazilian Portuguese), `es` (Spanish), `gn` (Guarani).

## Address Type Translations

| Value      | EN          | PT-BR     | ES           | GN           |
| ---------- | ----------- | --------- | ------------ | ------------ |
| billing    | Billing     | Cobrança  | Facturación  | Jehepyme'ã   |
| shipping   | Shipping    | Entrega   | Envío        | Mondo        |
| both       | Both        | Ambos     | Ambos        | Mokõive      |
| fiscal     | Fiscal      | Fiscal    | Fiscal       | Fiscal       |
| additional | Additional  | Adicional | Adicional    | Oĩmbýva      |

## Related

- [Address Store](./AddressStore.md)
- [Address Update](./AddressUpdate.md)

## Changelog

- 2025-10-23: Updated field name from `title` to `title_localized`.
- 2025-10-23: Initial documentation.
