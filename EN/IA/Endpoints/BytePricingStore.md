# IA — Byte Pricing: Store

Creates a byte-priced product with localized content and a single active price.

## Endpoint

```
POST /ia/admin/pricing/bytes
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

### Query parameters

None.

### Request body

```json
{
  "price": 100,
  "currency": "USD",
  "description": "Price per byte"
}
```

| Field        | Type    | Required | Description |
| ------------ | ------- | -------- | ----------- |
| price        | integer | Yes      | Price value (smallest unit; e.g., cents). |
| currency     | string  | Yes      | Currency code (USD, EUR, BRL, PYG). |
| description  | string  | No       | Default description content. |

## Examples

### Request (curl)

```bash
curl -X POST \
  -H "Authorization: Bearer <token>" \
  -H "Accept-Language: en" \
  -H "Content-Type: application/json" \
  -d '{"price":100,"description":"Price per byte"}' \
  "https://sandbox.your-domain.com/ia/admin/pricing/bytes"
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
    "description": "Price per byte",
    "language": "en",
    "price": 100,
    "currency": "USD",
    "formatted_price": "$1.00",
    "created_at": "2025-09-26T04:46:04-03:00"
  }
}
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
| data.description              | string|null | Default description content |
| data.language                 | string|null | Locale tied to the default title |
| data.price                    | integer     | Default price in minor units (e.g., cents) |
| data.currency                 | string      | ISO currency code |
| data.formatted_price          | string|null | Human-friendly currency string |
| data.created_at               | string|null | Creation timestamp (ISO 8601) |

## HTTP Status

- 200: OK
- 400: Bad Request
- 401: Unauthorized
- 422: Unprocessable Entity
- 500: Internal Server Error

## Errors

```json
{
  "message": "The given data was invalid.",
  "errors": { "price": ["The price field is required."] }
}
```

## Notes

- Title is seeded from translation key `common.byte_price`; slug remains `byte_price` to integrate with the Data Calculator pipeline.
- Title is immutable for Byte Pricing endpoints.

## Related

- docs/EN/IA/Endpoints/BytePricingIndex.md
- docs/EN/IA/Endpoints/BytePricingShow.md
- docs/EN/IA/Endpoints/BytePricingUpdate.md
- docs/EN/IA/Endpoints/BytePricingDestroy.md

## Changelog

- 2025-09-23: Aligned with single-price input and immutable title.
- 2025-09-26: Updated response payload and status to match implementation.
