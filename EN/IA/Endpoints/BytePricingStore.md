# IA — Byte Pricing: Store

## Endpoint

`POST /ia/admin/pricing/bytes`

Creates a product measured in BYTE with title, description and prices for all currencies.

---

## Authentication

Required — `auth:sanctum`.

### Required Headers
| Header | Type | Description |
| ------ | ---- | ----------- |
| Authorization | `string` | `Bearer <token>` required. |
| Accept-Language | `string` | Optional locale. |

---

## Body (JSON)
| Field | Type | Required | Description |
| ----- | ---- | -------- | ----------- |
| `title` | `string` | Yes | Default title. |
| `description` | `string` | No | Default description. |
| `prices` | `array<object>` | Yes | Each item must have `currency` (BRL/USD/EUR/PYG) and `value` (integer, cents). Must include all currencies. |

### Example
```json
{
  "title": "Payload Pricing",
  "description": "Price per byte",
  "prices": [
    { "currency": "USD", "value": 100 },
    { "currency": "EUR", "value": 90 },
    { "currency": "BRL", "value": 500 },
    { "currency": "PYG", "value": 70000 }
  ]
}
```

### Response (201)
```json
{ "data": { "uuid": "...", "measurement_type": "byte" } }
```

### Validation Error (422)
Missing currencies or invalid values.
