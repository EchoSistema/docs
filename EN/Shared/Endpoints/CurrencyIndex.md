# Shared – Available Currencies Index

## Endpoint

`GET /api/v1/currencies`

Returns the supported currencies for the backoffice, including each currency symbol (`sign`) and native name (`native_name`). Allows filtering by type (`fiat` or `crypto`).

---

## Authentication

None.

---

## Request

### Query Parameters

| Parameter | Type | Required | Description |
| --------- | ---- | -------- | ----------- |
| `type` | `string` | No | Filters the result to `fiat` or `crypto`. Comparison is case-insensitive. When absent or invalid, all currencies are returned. |

> The parameter must be sent as `type` (snake_case). Alternative casings are not recognized.

---

## Example Response

```json
{
  "data": [
    {
      "name": "BRL",
      "value": 1,
      "sign": "R$",
      "native_name": "Real Brasileiro"
    },
    {
      "name": "BTC",
      "value": 7,
      "sign": "₿",
      "native_name": "Bitcoin"
    }
  ]
}
```

---

## JSON Structure Explanation

| Field | Type | Description |
| ----- | ---- | ----------- |
| `data[]` | `array` | List of available currencies. |
| `data[].name` | `string` | Currency code (enum name). |
| `data[].value` | `integer` | Numeric enum value. |
| `data[].sign` | `string` | Currency symbol or ticker. |
| `data[].native_name` | `string` | Currency native name. |

---

## Notes

* `type=fiat` returns only fiat currencies; `type=crypto` returns only crypto assets.
* Missing or invalid values fall back to the full list.
* Ordering follows the declaration order in `CurrencyEnum`.

---

## Changelog

- 2025-10-03: Initial documentation for the endpoint with type filtering (`fiat`/`crypto`).
