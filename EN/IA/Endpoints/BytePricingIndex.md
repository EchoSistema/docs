# IA — Byte Pricing: Index

## Endpoint

`GET /ia/admin/pricing/bytes`

Returns a paginated list of byte-pricing products (measurement_type = BYTE) with title, description and prices.

---

## Authentication

Required — `auth:sanctum`.

### Required Headers
| Header | Type | Description |
| ------ | ---- | ----------- |
| Authorization | `string` | `Bearer <token>` required. |
| Accept-Language | `string` | Optional locale. |

---

## Response

```json
{
  "data": [
    {
      "uuid": "...",
      "measurement_type": "byte",
      "title": { "content": "Payload Pricing" },
      "text": { "content": "Price per byte" },
      "prices": [
        { "currency_id": 2, "value": 123 },
        { "currency_id": 3, "value": 456 }
      ]
    }
  ],
  "meta": { "current_page": 1, "per_page": 25 }
}
```

---

## Notes
- Only products with `measurement_type = byte` are returned.

