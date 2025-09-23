# Addresses — Update

## Endpoint

`PUT /ia/admin/addresses/{address}` (IA Admin)

Updates an existing address. Only provided fields are changed; `uuid` is prohibited.

---

## Authentication

`auth:sanctum`.

### Required Headers
| Header | Type | Description |
| ------ | ---- | ----------- |
| Authorization | `string` | `Bearer <token>` required. |
| Accept-Language | `string` | Locale (e.g., `en`, `pt-BR`). Optional. |

---

## Body (JSON)
| Field           | Type              | Req. | Description |
| --------------- | ----------------- | ---- | ----------- |
| `type`          | `AddressTypeEnum` | No   | Address type. |
| `city`          | `string|object`   | No   | City reference or use IDs below. |
| `city_id`       | `integer`         | No   | City ID. |
| `state_id`      | `integer`         | No   | State ID. |
| `country_id`    | `integer`         | No   | Country ID. |
| `zipcode`       | `string`          | No   | Postal code. |
| `address_one`   | `string`          | No   | Address line 1. |
| `address_two`   | `string`          | No   | Address line 2. |
| `address_three` | `string`          | No   | Address line 3. |
| `address_four`  | `string`          | No   | Address line 4. |
| `uuid`          | —                 | —    | Prohibited. |

### Success (200)
Returns the updated address resource.

### Validation Error (422)
```json
{ "message": "The given data was invalid.", "errors": { "uuid": ["The uuid field is prohibited."], "city_id": ["The city id must be an integer."], "zipcode": ["The zipcode must be a string."] } }
```

### Not Found (404)
```json
{ "message": "Not Found" }
```

### Unauthorized (401)
```json
{ "message": "Unauthenticated." }
```

### Forbidden (403)
```json
{ "message": "This action is unauthorized." }
```

---

## Notes
- Localisation: city/state/country names may follow `Accept-Language`.

