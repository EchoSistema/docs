# IA — Byte Pricing: Show

Returns the platform byte-priced product with its localized metadata and default price per byte.

## Endpoint

```
GET /ia/admin/pricing/bytes/details
```

## Authentication

Required — Bearer {token} with Sanctum.

## Headers

| Header          | Type   | Required | Description |
| --------------- | ------ | -------- | ----------- |
| Authorization   | string | Yes      | `Bearer {token}` |
| Accept-Language | string | No       | IETF locale |

## Parameters

### Path parameters

None.

## Examples

### Request (curl)

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "Accept-Language: en" \
  "https://sandbox.your-domain.com/ia/admin/pricing/bytes/details"
```

### Response (200)

```json
{
  "data": {
    "uuid": "9e3c5352-a2d7-411d-9ba5-c29756966ca7",
    "measurement_type": {
      "id": "byte",
      "name": "BYTE",
      "title": "Byte"
    },
    "title": "Price per Byte",
    "slug": "byte_price",
    "description": "Default byte pricing description",
    "language": "en",
    "price": "0.0299",
    "raw_price": 299,
    "price_precision": 4,
    "prices": [
      {
        "currency_id": 840,
        "currency": "USD",
        "value": "0.0055",
        "raw_value": 55,
        "formatted_value": "$0.0055"
      }
    ],
    "currency": "BRL",
    "formatted_price": "R$\u00a00.0299",
    "created_at": "2025-09-26T04:46:04-03:00"
  }
}
```

## JSON Structure Explained

| Field                          | Type        | Description |
| ------------------------------ | ----------- | ----------- |
| data.uuid                      | string      | Product identifier |
| data.measurement_type          | object      | Measurement type metadata |
| data.measurement_type.id       | string      | Enum identifier (`byte`) |
| data.measurement_type.name     | string      | Enum name (`BYTE`) |
| data.measurement_type.title    | string      | Human readable label |
| data.title                     | string|null | Localized product title |
| data.slug                      | string|null | Slug used internally to locate the product |
| data.description               | string|null | Default description content |
| data.language                  | string|null | Locale tied to the default title |
| data.price                     | string|null | Price per byte formatted with four decimal places |
| data.raw_price                 | integer|null | Original stored amount in minor units (e.g., cents) |
| data.price_precision           | integer|null | Decimal precision applied to `price` and formatted outputs |
| data.prices                    | array       | Alternate active prices per currency |
| data.prices[].currency_id      | integer     | Currency enum identifier |
| data.prices[].currency         | string      | Currency ISO code |
| data.prices[].value            | string      | Alternate price per byte with four decimals |
| data.prices[].raw_value        | integer     | Stored alternate price in minor units |
| data.prices[].formatted_value  | string      | Alternate currency string respecting four decimals |
| data.currency                  | string|null | ISO currency code of the default price |
| data.formatted_price           | string|null | Default price formatted with four decimal places |
| data.created_at                | string|null | Creation timestamp (ISO 8601) |

## HTTP Status

- 200: OK
- 401: Unauthorized
- 404: Not Found (when the bound product is not BYTE)

## Errors

```json
{ "message": "Not Found" }
```

## Related

- docs/EN/IA/Endpoints/BytePricingIndex.md
- docs/EN/IA/Endpoints/BytePricingStore.md
- docs/EN/IA/Endpoints/BytePricingUpdate.md
- docs/EN/IA/Endpoints/BytePricingDestroy.md

## Changelog

- 2025-09-23: Clarified response fields and status codes.
- 2025-10-03: Documented per-byte precision fields, alternate prices, and new details endpoint without path parameters.
- 2025-09-26: Updated response payload example to match current resource shape.
