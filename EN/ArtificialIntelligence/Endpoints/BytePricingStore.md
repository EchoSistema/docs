# Artificial Intelligence – Create Byte Pricing Product

## Endpoint

```
POST /api/v1/ai/admin/pricing/bytes
```

## Authentication

Required – Bearer {token} with ability `auth:sanctum`

## Headers

| Header           | Type   | Required | Description |
| ---------------- | ------ | -------- | ----------- |
| Authorization    | string | Yes      | `Bearer {token}`. |
| X-PUBLIC-KEY     | string | Yes      | Platform public key. |
| Accept-Language  | string | No       | IETF locale (e.g., `pt-BR`, `en`, `es`). |
| Content-Type     | string | Yes      | `application/json`. |

## Parameters

### Body parameters

| Parameter | Type    | Required | Description | Default/Values |
| --------- | ------- | -------- | ----------- | -------------- |
| price     | integer | Yes      | Price per byte (raw value * 10000). For example, 10 represents $0.0010 | Min: 0 |
| currency  | string  | Yes      | Currency code (ISO 4217). | Valid currency names from CurrencyEnum |
| description | string | No     | Product description. | Max: 255 characters |
| language  | string  | No       | Language code for the title. | Platform default language |

> Parameter names accept `snake_case`, `camelCase`, `kebab-case`, or `CapitalCase` variants.

## Examples

### Request example (curl)

```bash
curl -X POST \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: en" \
  -H "Content-Type: application/json" \
  -d '{
    "price": 10,
    "currency": "USD",
    "description": "Price per byte for data processing and storage",
    "language": "en"
  }' \
  "https://sandbox.your-domain.com/api/v1/ai/admin/pricing/bytes"
```

### Request example (JavaScript)

```javascript
const response = await fetch('https://sandbox.your-domain.com/api/v1/ai/admin/pricing/bytes', {
  method: 'POST',
  headers: {
    'Authorization': 'Bearer <token>',
    'X-PUBLIC-KEY': '<key>',
    'Accept-Language': 'en',
    'Content-Type': 'application/json',
  },
  body: JSON.stringify({
    price: 10,
    currency: 'USD',
    description: 'Price per byte for data processing and storage',
    language: 'en'
  })
});
```

### Response example

```json
{
  "data": {
    "uuid": "00000000-0000-0000-0000-000000000001",
    "measurement_type": {
      "id": 1,
      "name": "BYTE",
      "title": "Byte"
    },
    "title": "Byte Price",
    "slug": "byte_price",
    "description": "Price per byte for data processing and storage",
    "language": "en",
    "price": "0.0010",
    "raw_price": 10,
    "price_precision": 4,
    "prices": [],
    "currency": "USD",
    "formatted_price": "$0.0010",
    "created_at": "2025-10-23T14:30:00Z"
  }
}
```

## JSON Structure Explained

| Field                          | Type    | Description |
| ------------------------------ | ------- | ----------- |
| data                           | object  | Created byte pricing product. |
| data.uuid                      | string  | Product UUID. |
| data.measurement_type          | object  | Measurement type information. |
| data.measurement_type.id       | integer | Measurement type ID. |
| data.measurement_type.name     | string  | Measurement type name (BYTE). |
| data.measurement_type.title    | string  | Human-readable measurement type. |
| data.title                     | string  | Product title (always "Byte Price"). |
| data.slug                      | string  | URL-friendly identifier (always "byte_price"). |
| data.description               | string  | Product description. |
| data.language                  | string  | Content language code. |
| data.price                     | string  | Price per byte (formatted with 4 decimal places). |
| data.raw_price                 | integer | Raw price value (price * 10000). |
| data.price_precision           | integer | Decimal precision for price (always 4). |
| data.prices                    | array   | Alternative prices (empty on creation). |
| data.currency                  | string  | Currency code. |
| data.formatted_price           | string  | Formatted price with currency symbol. |
| data.created_at                | string  | Creation timestamp (ISO 8601). |

## HTTP Status

- 200: OK (product already exists, returns existing product)
- 201: Created
- 401: Unauthorized
- 403: Forbidden
- 422: Unprocessable Entity (validation errors)
- 429: Too Many Requests
- 500: Internal Server Error

## Errors

### Validation Error

```json
{
  "message": "The price field is required.",
  "errors": {
    "price": [
      "The price field is required."
    ]
  }
}
```

### Unauthenticated

```json
{
  "message": "Unauthenticated.",
  "errors": {}
}
```

## Notes

- This endpoint creates a byte pricing product for the authenticated user's platform.
- The title is automatically set to "Byte Price" and slug to "byte_price".
- The measurement type is automatically set to BYTE.
- If a byte pricing product already exists for the platform, the existing product is returned instead of creating a new one.
- The price parameter should be the raw value multiplied by 10000 (e.g., to set a price of $0.0010, send 10).
- The currency parameter must be a valid currency name from the CurrencyEnum (e.g., "USD", "EUR", "GBP").
- The description is optional and can be up to 255 characters.

## Related

- [Byte Pricing Index](./BytePricingIndex.md)
- [Byte Pricing Show](./BytePricingShow.md)
- [Byte Pricing Update](./BytePricingUpdate.md)
- [Byte Pricing Destroy](./BytePricingDestroy.md)

## Changelog

- 2025-10-23: Updated with detailed documentation and accurate request/response structure.
- 2025-10-16: Initial documentation.
