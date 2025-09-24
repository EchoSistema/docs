# IA — Byte Pricing: Update

## Endpoint

`PUT /ia/admin/pricing/bytes/{product}`

Updates title, description and/or prices of a BYTE product.

---

## Body (JSON)
| Field | Type | Required | Description |
| ----- | ---- | -------- | ----------- |
| `title` | `string` | No | Default title. |
| `description` | `string` | No | Default description. |
| `prices` | `array<object>` | No | Each item with `currency` (BRL/USD/EUR/PYG) and `value` (integer, cents). Partial updates allowed. |

### Response (200)
```json
{ "data": { "uuid": "...", "measurement_type": "byte" } }
```
