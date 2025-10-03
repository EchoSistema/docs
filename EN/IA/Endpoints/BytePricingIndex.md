# IA — Byte Pricing: Index

Lists byte-priced products with localized content and their default prices per byte.

## Endpoint

```
GET /ia/admin/pricing/bytes
```

## Authentication

Required — Bearer {token} with Sanctum.

## Headers

| Header          | Type   | Required | Description |
| --------------- | ------ | -------- | ----------- |
| Authorization   | string | Yes      | `Bearer {token}` |
| Accept-Language | string | No       | IETF locale (`en`, `pt-BR`, `es`, ...) |

## Parameters

### Path parameters

None.

### Query parameters

| Parameter | Type   | Required | Description | Default/Values |
| --------- | ------ | -------- | ----------- | -------------- |
| page      | int    | No       | Page number | 1 |

## Examples

### Request (curl)

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "Accept-Language: en" \
  "https://sandbox.your-domain.com/ia/admin/pricing/bytes?page=1"
```

### Response (200)

```json
{
  "data": [
    {
      "uuid": "9e3c5352-a2d7-411d-9ba5-c29756966ca7",
      "measurement_type": {
        "id": "byte",
        "name": "BYTE",
        "title": "Byte"
      },
      "title": "Price per Byte",
      "slug": "byte_price",
      "description": null,
      "language": "en",
      "price": "0.0299",
      "raw_price": 299,
      "price_precision": 4,
      "prices": [],
      "currency": "BRL",
      "formatted_price": "R$\u00a00.0299",
      "created_at": "2025-09-26T04:46:04-03:00"
    }
  ],
  "links": {
    "first": "https://sandbox.your-domain.com/ia/admin/pricing/bytes?page=1",
    "last": "https://sandbox.your-domain.com/ia/admin/pricing/bytes?page=1",
    "prev": null,
    "next": null
  },
  "meta": {
    "current_page": 1,
    "from": 1,
    "last_page": 1,
    "links": [
      {
        "url": null,
        "label": "\u00ab Previous",
        "active": false
      },
      {
        "url": "https://sandbox.your-domain.com/ia/admin/pricing/bytes?page=1",
        "label": "1",
        "active": true
      },
      {
        "url": null,
        "label": "Next \u00bb",
        "active": false
      }
    ],
    "path": "https://sandbox.your-domain.com/ia/admin/pricing/bytes",
    "per_page": 25,
    "to": 1,
    "total": 1
  }
}
```

## JSON Structure Explained

| Field                                | Type        | Description |
| ------------------------------------ | ----------- | ----------- |
| data[]                               | array       | Paginated list of byte-priced products |
| data[].uuid                          | string      | Product identifier |
| data[].measurement_type              | object      | Measurement type metadata |
| data[].measurement_type.id           | string      | Enum identifier (`byte`) |
| data[].measurement_type.name         | string      | Enum name (`BYTE`) |
| data[].measurement_type.title        | string      | Human readable label |
| data[].title                         | string|null | Localized product title |
| data[].slug                          | string|null | Slug used to reference the product (expected `byte_price`) |
| data[].description                   | string|null | Optional localized description |
| data[].language                      | string|null | Locale tied to the default title |
| data[].price                         | string|null | Default price per byte formatted with four decimal places |
| data[].raw_price                     | integer|null | Original stored amount in minor units (e.g., cents) |
| data[].price_precision               | integer|null | Decimal precision applied to price formatting |
| data[].prices                        | array       | Alternate active prices per currency |
| data[].prices[].currency_id          | integer     | Currency enum identifier |
| data[].prices[].currency             | string      | Currency ISO code |
| data[].prices[].value                | string      | Alternate price per byte with four decimals |
| data[].prices[].raw_value            | integer     | Stored alternate price in minor units |
| data[].prices[].formatted_value      | string      | Alternate currency string respecting four decimals |
| data[].currency                      | string|null | ISO currency code |
| data[].formatted_price               | string|null | Human-friendly currency string with four decimals |
| data[].created_at                    | string|null | Creation timestamp (ISO 8601) |
| links                                | object      | Pagination navigation links |
| meta                                 | object      | Pagination metadata |

## HTTP Status

- 200: OK
- 401: Unauthorized
- 429: Too Many Requests
- 500: Internal Server Error

## Errors

```json
{ "message": "[message]" }
```

## Notes

- Returns only products stored with measurement type `byte`.
- Pagination link labels follow the backend locale.
- Page size is fixed to 25 records.
- The collection payload may include additional keys when the product `raw` attribute is populated.

## Related

- docs/EN/IA/Endpoints/BytePricingShow.md
- docs/EN/IA/Endpoints/BytePricingStore.md
- docs/EN/IA/Endpoints/BytePricingUpdate.md
- docs/EN/IA/Endpoints/BytePricingDestroy.md

## Changelog

- 2025-09-23: Updated to single-price model and immutable title.
- 2025-10-03: Documented per-byte precision fields and alternate prices.
- 2025-09-26: Refreshed response example and field descriptions.
