# IA — Byte Pricing: Update

Updates the default description and/or single price of an existing byte-priced product.

## Endpoint

```
PUT /ia/admin/pricing/bytes/{product}
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

| Parameter | Type   | Required | Description |
| --------- | ------ | -------- | ----------- |
| product   | string | Yes      | Product route key |

### Request body

```json
{
  "price": 150,
  "currency": "EUR",
  "description": "New description"
}
```

| Field       | Type    | Required | Description |
| ----------- | ------- | -------- | ----------- |
| price       | integer | No       | New price value (smallest unit; e.g., cents). |
| currency    | string  | No (required with price) | Currency code (USD, EUR, BRL, PYG). |
| description | string  | No       | New default description. |

## Examples

### Request (curl)

```bash
curl -X PUT \
  -H "Authorization: Bearer <token>" \
  -H "Accept-Language: en" \
  -H "Content-Type: application/json" \
  -d '{"price":150,"description":"New description"}' \
  "https://sandbox.your-domain.com/ia/admin/pricing/bytes/{product}"
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
    "description": "New description",
    "language": "en",
    "price": 150,
    "currency": "EUR",
    "formatted_price": "€1.50",
    "created_at": "2025-09-26T04:46:04-03:00"
  }
}
```

## HTTP Status

- 200: OK
- 400: Bad Request
- 401: Unauthorized
- 404: Not Found (when product is not BYTE)
- 422: Unprocessable Entity

## Errors

```json
{ "message": "[message]" }
```

## JSON Structure Explained

| Field                         | Type        | Description |
| ----------------------------- | ----------- | ----------- |
| data.uuid                     | string      | Product identifier |
| data.measurement_type         | object      | Measurement type metadata |
| data.measurement_type.id      | string      | Enum identifier (`byte`) |
| data.measurement_type.name    | string      | Enum name (`BYTE`) |
| data.measurement_type.title   | string      | Human readable label |
| data.title                    | string|null | Localized product title |
| data.slug                     | string|null | Slug used internally to locate the product |
| data.description              | string|null | Updated default description |
| data.language                 | string|null | Locale tied to the default title |
| data.price                    | integer|null | Updated price in minor units (if provided) |
| data.currency                 | string|null | ISO currency code |
| data.formatted_price          | string|null | Human-friendly currency string |
| data.created_at               | string|null | Creation timestamp (ISO 8601) |

## Notes

- Title for Byte Pricing is immutable and will not be updated by this endpoint.
- Price and currency must be provided together; omitting both keeps the existing values.

## Related

- docs/EN/IA/Endpoints/BytePricingIndex.md
- docs/EN/IA/Endpoints/BytePricingShow.md
- docs/EN/IA/Endpoints/BytePricingStore.md
- docs/EN/IA/Endpoints/BytePricingDestroy.md

## Changelog

- 2025-09-23: Updated for single-price updates and immutable title.
- 2025-09-26: Refreshed response payload and field reference.
