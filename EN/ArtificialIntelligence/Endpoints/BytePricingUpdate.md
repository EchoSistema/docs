# Artificial Intelligence – Update Byte Pricing Product

## Endpoint

```
PUT /api/v1/ai/admin/pricing/bytes/{product:uuid}
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

### Path parameters

| Parameter | Type   | Required | Description |
| --------- | ------ | -------- | ----------- |
| product   | string | Yes      | Product UUID. |

### Body parameters

| Parameter   | Type    | Required | Description | Default/Values |
| ----------- | ------- | -------- | ----------- | -------------- |
| price       | integer | No       | Price per byte (raw value * 10000). | Min: 0 |
| currency    | string  | Required with price | Currency code (ISO 4217). | Valid currency names from CurrencyEnum |
| description | string  | No       | Product description. | Max: 5000 characters |

> Parameter names accept `snake_case`, `camelCase`, `kebab-case`, or `CapitalCase` variants.

## Examples

### Request example (curl)

```bash
curl -X PUT \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: en" \
  -H "Content-Type: application/json" \
  -d '{
    "price": 15,
    "currency": "USD",
    "description": "Updated price per byte for enhanced data processing"
  }' \
  "https://sandbox.your-domain.com/api/v1/ai/admin/pricing/bytes/00000000-0000-0000-0000-000000000001"
```

### Request example (JavaScript)

```javascript
const response = await fetch('https://sandbox.your-domain.com/api/v1/ai/admin/pricing/bytes/00000000-0000-0000-0000-000000000001', {
  method: 'PUT',
  headers: {
    'Authorization': 'Bearer <token>',
    'X-PUBLIC-KEY': '<key>',
    'Accept-Language': 'en',
    'Content-Type': 'application/json',
  },
  body: JSON.stringify({
    price: 15,
    currency: 'USD',
    description: 'Updated price per byte for enhanced data processing'
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
    "description": "Updated price per byte for enhanced data processing",
    "language": "en",
    "price": "0.0015",
    "raw_price": 15,
    "price_precision": 4,
    "prices": [
      {
        "currency_id": 2,
        "currency": "EUR",
        "value": "0.0013",
        "raw_value": 13,
        "formatted_value": "€0.0013"
      }
    ],
    "currency": "USD",
    "formatted_price": "$0.0015",
    "created_at": "2025-10-16T10:30:00Z"
  }
}
```

## JSON Structure Explained

| Field                          | Type    | Description |
| ------------------------------ | ------- | ----------- |
| data                           | object  | Updated byte pricing product. |
| data.uuid                      | string  | Product UUID. |
| data.measurement_type          | object  | Measurement type information. |
| data.measurement_type.id       | integer | Measurement type ID. |
| data.measurement_type.name     | string  | Measurement type name (BYTE). |
| data.measurement_type.title    | string  | Human-readable measurement type. |
| data.title                     | string  | Product title. |
| data.slug                      | string  | URL-friendly identifier. |
| data.description               | string  | Product description. |
| data.language                  | string  | Content language code. |
| data.price                     | string  | Price per byte (formatted with 4 decimal places). |
| data.raw_price                 | integer | Raw price value (price * 10000). |
| data.price_precision           | integer | Decimal precision for price (always 4). |
| data.prices                    | array   | Alternative prices in other currencies. |
| data.prices[].currency_id      | integer | Currency ID. |
| data.prices[].currency         | string  | Currency code (ISO 4217). |
| data.prices[].value            | string  | Price value in this currency. |
| data.prices[].raw_value        | integer | Raw price value. |
| data.prices[].formatted_value  | string  | Formatted price with currency symbol. |
| data.currency                  | string  | Default currency code. |
| data.formatted_price           | string  | Formatted default price with currency symbol. |
| data.created_at                | string  | Creation timestamp (ISO 8601). |

## HTTP Status

- 200: OK
- 401: Unauthorized
- 403: Forbidden
- 404: Not Found (product not found or wrong measurement type)
- 422: Unprocessable Entity (validation errors)
- 429: Too Many Requests
- 500: Internal Server Error

## Errors

### Validation Error

```json
{
  "message": "The currency field is required when price is present.",
  "errors": {
    "currency": [
      "The currency field is required when price is present."
    ]
  }
}
```

### Not Found

```json
{
  "message": "The requested resource was not found.",
  "errors": {}
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

- This endpoint updates a byte pricing product.
- The product must have measurement type BYTE, otherwise 404 is returned.
- All parameters are optional, allowing partial updates.
- If you provide a price, you must also provide the currency.
- The price parameter should be the raw value multiplied by 10000 (e.g., to set a price of $0.0015, send 15).
- The description can be up to 5000 characters (increased from 255 in store).
- The currency parameter must be a valid currency name from the CurrencyEnum.

## Related

- [Byte Pricing Index](./BytePricingIndex.md)
- [Byte Pricing Show](./BytePricingShow.md)
- [Byte Pricing Store](./BytePricingStore.md)
- [Byte Pricing Destroy](./BytePricingDestroy.md)

## Changelog

- 2025-10-23: Updated with detailed documentation and accurate request/response structure.
- 2025-10-16: Initial documentation.
