# Shared – Update user currency

## Endpoint

```
PATCH /api/v1/user/currency
```

Updates the preferred currency for the authenticated user, ensuring the provided value matches `CurrencyEnum`.

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
  "currency": "BRL"
}
```

| Field    | Type   | Required | Description |
| -------- | ------ | -------- | ----------- |
| currency | string | Yes      | Enum name from `CurrencyEnum` (e.g., `BRL`, `USD`, `EUR`, `PYG`, `USDC`, `USDT`, `BTC`, `ETH`, `DOGE`, `XMR`, `SOL`). |

## Examples

### Request (curl)

```bash
curl -X PATCH \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: en" \
  -H "Content-Type: application/json" \
  -d '{"currency":"BRL"}' \
  "https://sandbox.echosistema.com/api/v1/user/currency"
```

### Success response

```json
{
  "data": {
    "currency": "BRL"
  },
  "meta": []
}
```

## JSON Structure Explained

| Field         | Type   | Description |
| ------------- | ------ | ----------- |
| data.currency | string | Currency stored for the user. |
| meta          | array  | Reserved for metadata (empty array). |

## HTTP Status

- 200: Currency updated successfully.
- 401: Missing or invalid token.
- 422: `currency` not recognized by the enum.
- 500: Unexpected server error.

## Errors

```json
{
  "message": "The selected currency is invalid.",
  "errors": {
    "currency": [
      "The currency field is invalid."
    ]
  }
}
```

## Notes

- Route name: `api.v1.user.currency.update`.
- The currency is stored in `regional_information.currency_id` using `CurrencyEnum`.
- A regional information record is created automatically when missing.

## Related

- [Shared – Update user language](UserLanguageUpdate.md)
- [Shared – Available currencies](CurrencyIndex.md)

## Changelog

- 2025-10-14: Endpoint documented.
