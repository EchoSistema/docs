# Shared – Available Currencies Index

## Endpoint

```
GET /api/v1/currencies
```

Returns the backoffice currencies defined in `CurrencyEnum`, exposing the symbol (`sign`) and native name (`native_name`).

---

## Authentication

None.

---

## Headers

| Header          | Type   | Required | Description |
| --------------- | ------ | -------- | ----------- |
| X-PUBLIC-KEY    | string | No       | Include when the frontend already sends it by convention. |
| Accept-Language | string | No       | IETF locale for translated texts (`pt-BR`, `en`, `es`). |

---

## Parameters

### Query Parameters

| Parameter | Type   | Required | Description |
| --------- | ------ | -------- | ----------- |
| `type`    | string | No       | Filters the result to `fiat` or `crypto`. Case-insensitive. Missing or invalid values return all currencies. |

> Canonical name is `type` in snake_case. Other casings are not accepted.

---

## Examples

### Sample request (curl)

```bash
curl -X GET \
  "https://sandbox.example.com/api/v1/currencies?type=crypto"
```

### Sample response

```json
{
  "data": [
    {
      "name": "BTC",
      "value": 7,
      "sign": "₿",
      "native_name": "Bitcoin"
    },
    {
      "name": "ETH",
      "value": 8,
      "sign": "Ξ",
      "native_name": "Ethereum"
    }
  ]
}
```

---

## JSON Structure Explanation

| Field              | Type    | Description |
| ------------------ | ------- | ----------- |
| `data[]`           | array   | List of available currencies. |
| `data[].name`      | string  | Enum name (`BRL`, `USD`, `BTC`...). |
| `data[].value`     | integer | Enum integer value. |
| `data[].sign`      | string  | Currency symbol or ticker. |
| `data[].native_name` | string | Currency native name. |

---

## HTTP Status

- 200: Success
- 400: Invalid request (e.g., malformed query)
- 500: Internal error

---

## Notes

- `type=fiat` returns only fiat currencies; `type=crypto` returns crypto assets.
- Missing or unsupported values fall back to the complete list via `CurrencyEnum::cases()`.
- Ordering matches the enum declaration order.

---

## Changelog

- 2025-10-03: Documentation refreshed to reflect the type filter and standard structure.
