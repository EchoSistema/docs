# IA — Byte Pricing: Show

## Endpoint

`GET /ia/admin/pricing/bytes/{product}`

Returns a single byte-pricing product.

---

## Authentication

Required — `auth:sanctum`.

### Required Headers
| Header | Type | Description |
| ------ | ---- | ----------- |
| Authorization | `string` | `Bearer <token>` required. |
| Accept-Language | `string` | Optional locale. |

---

## Response (200)

```json
{
  "data": {
    "uuid": "...",
    "measurement_type": "byte",
    "title": { "content": "Payload Pricing" },
    "text": { "content": "Price per byte" },
    "prices": [ { "currency_id": 2, "value": 123 } ]
  }
}
```

### Not Found (404)
When the product is not of type BYTE.

