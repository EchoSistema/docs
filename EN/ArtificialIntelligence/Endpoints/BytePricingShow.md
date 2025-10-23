# Artificial Intelligence – Show Byte Pricing Details

## Endpoint

```
GET /api/v1/ai/admin/pricing/bytes/details
```

## Authentication

Required – Bearer {token} with ability `auth:sanctum`

## Headers

| Header           | Type   | Required | Description |
| ---------------- | ------ | -------- | ----------- |
| Authorization    | string | Yes      | `Bearer {token}`. |
| X-PUBLIC-KEY     | string | Yes      | Platform public key. |
| Accept-Language  | string | No       | IETF locale (e.g., `pt-BR`, `en`, `es`). |

## Parameters

No parameters required.

## Examples

### Request example (curl)

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: en" \
  "https://sandbox.your-domain.com/api/v1/ai/admin/pricing/bytes/details"
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
    "description": "Price per byte for data processing",
    "language": "en",
    "price": "0.0010",
    "raw_price": 10,
    "price_precision": 4,
    "prices": [
      {
        "currency_id": 2,
        "currency": "EUR",
        "value": "0.0009",
        "raw_value": 9,
        "formatted_value": "€0.0009"
      },
      {
        "currency_id": 3,
        "currency": "GBP",
        "value": "0.0008",
        "raw_value": 8,
        "formatted_value": "£0.0008"
      }
    ],
    "currency": "USD",
    "formatted_price": "$0.0010",
    "created_at": "2025-10-16T10:30:00Z"
  }
}
```

## JSON Structure Explained

| Field                          | Type    | Description |
| ------------------------------ | ------- | ----------- |
| data                           | object  | Byte pricing product details. |
| data.uuid                      | string  | Product UUID. |
| data.measurement_type          | object  | Measurement type information. |
| data.measurement_type.id       | integer | Measurement type ID. |
| data.measurement_type.name     | string  | Measurement type name (BYTE). |
| data.measurement_type.title    | string  | Human-readable measurement type. |
| data.title                     | string  | Product title. |
| data.slug                      | string  | URL-friendly identifier (always "byte_price"). |
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
- 404: Not Found (when byte pricing product doesn't exist for the platform)
- 429: Too Many Requests
- 500: Internal Server Error

## Errors

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

- This endpoint returns the byte pricing product associated with the authenticated user's platform.
- The product is identified by the slug "byte_price" and measurement type BYTE.
- Returns 404 if no byte pricing product exists for the platform.
- The price represents the cost per byte of data processing.
- The `formatted_price` uses the platform's locale for currency formatting.
- Alternative prices show all active prices in currencies different from the default currency.

## Related

- [Byte Pricing Index](./BytePricingIndex.md)
- [Byte Pricing Store](./BytePricingStore.md)
- [Byte Pricing Update](./BytePricingUpdate.md)
- [Byte Pricing Destroy](./BytePricingDestroy.md)
- [Data Calculator Process](./DataCalculatorProcess.md)

## Changelog

- 2025-10-23: Updated with detailed documentation and accurate response structure.
- 2025-10-16: Initial documentation.
