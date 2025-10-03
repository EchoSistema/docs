# Shared – Available Currencies Index

## Endpoint

```
GET /api/v1/currencies
```

Returns the currencies supported by the backoffice grouped into `fiat` and `crypto` buckets derived from `CurrencyEnum`.

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
| `type`    | string | No       | Filters the result to `fiat` or `crypto`. Case-insensitive. When provided, the opposite group is returned as `null`. Missing or invalid values return both groups. |

> Canonical name is `type` (snake_case). Other casings are not supported.

---

## Examples

### Sample request (curl)

```bash
curl -X GET \
  "https://sandbox.example.com/api/v1/currencies?type=fiat"
```

### Sample response

```json
{
  "data": {
    "crypto": null,
    "fiat": [
      {
        "name": "BRL",
        "value": 1,
        "sign": "R$",
        "native_name": "Real Brasileiro",
        "type": "fiat"
      },
      {
        "name": "USD",
        "value": 2,
        "sign": "$",
        "native_name": "US Dollar",
        "type": "fiat"
      }
    ]
  }
}
```

---

## JSON Structure Explanation

| Field                      | Type            | Description |
| -------------------------- | --------------- | ----------- |
| `data.crypto`              | array \| null   | Crypto currencies when available. |
| `data.fiat`                | array \| null   | Fiat currencies when available. |
| `data.*[].name`            | string          | Enum name (`BRL`, `USD`, `BTC`...). |
| `data.*[].value`           | integer         | Enum integer value. |
| `data.*[].sign`            | string          | Currency symbol or ticker. |
| `data.*[].native_name`     | string          | Currency native name. |
| `data.*[].type`            | string          | Group label (`fiat` or `crypto`). |

---

## HTTP Status

- 200: Success
- 400: Invalid request (e.g., malformed query)
- 500: Internal error

---

## Notes

- `type=fiat` returns only the fiat array with `data.crypto = null`; `type=crypto` mirrors this behaviour.
- Without a filter, both groups are returned using `CurrencyEnum::getFiat()` and `CurrencyEnum::getCrypto()`.
- Ordering matches the enum declaration.

---

## Changelog

- 2025-10-03: Documentation updated to describe grouped response (`crypto`/`fiat`).
