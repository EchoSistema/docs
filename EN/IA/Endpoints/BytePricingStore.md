# IA — Byte Pricing: Store

## Endpoint

```
POST /ia/admin/pricing/bytes
```

Creates a product measured in BYTE with localized title, optional description, and a single price.

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

### Response (201)

```json
{
  "data": {
    "uuid": "...",
    "measurement_type": "byte",
    "title": { "content": "Price per Byte" },
    "text": { "content": "Price per byte" },
    "prices": [ { "currency_id": 2, "value": 100, "is_default": true } ]
  }
}
```

## JSON Structure Explained

| Field                  | Type    | Description |
| ---------------------- | ------- | ----------- |
| data.uuid              | string  | Product identifier |
| data.measurement_type  | string  | Measurement type (`byte`) |
| data.title.content     | string  | Localized default title |
| data.text.content      | string  | Default description |
| data.prices[]          | array   | Prices attached to product |

## HTTP Status

- 201: Created
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

- Title is created for all available languages from translation key `common.byte_price` and slug fixed to `byte_price`.
- Title is immutable for Byte Pricing endpoints.

## Related

- docs/EN/IA/Endpoints/BytePricingIndex.md
- docs/EN/IA/Endpoints/BytePricingShow.md
- docs/EN/IA/Endpoints/BytePricingUpdate.md
- docs/EN/IA/Endpoints/BytePricingDestroy.md

## Changelog

- 2025-09-23: Aligned with single-price input and immutable title.
