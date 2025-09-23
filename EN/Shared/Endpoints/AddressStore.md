# Addresses — Create

## Endpoint

`POST /addresses` (Backoffice)  |  `POST /ia/admin/addresses` (IA Admin)

Creates an address. When not using nested resources, `uuid` is required.

---

## Authentication

- `POST /addresses`: `auth:sanctum` with `ability:backoffice`.
- `POST /ia/admin/addresses`: `auth:sanctum`.

### Required Headers
| Header | Type | Description |
| ------ | ---- | ----------- |
| Authorization | `string` | `Bearer <token>` required. |
| Accept-Language | `string` | Locale (e.g., `en`, `pt-BR`). Optional. |

---

## Body (JSON)
| Field           | Type              | Req. | Description |
| --------------- | ----------------- | ---- | ----------- |
| `type`          | `AddressTypeEnum` | No   | Defaults to `both`. One of `billing`, `shipping`, `both`, `fiscal`, `additional`. |
| `uuid`          | `uuid`            | Cond | Required when not using nested `addressable` routes. |
| `city`          | `string|object`   | Yes  | City reference; resolved via pipeline. |
| `address_one`   | `string`          | Yes  | Address line 1. |
| `address_two`   | `string`          | No   | Address line 2. |
| `address_three` | `string`          | No   | Address line 3. |
| `address_four`  | `string`          | No   | Address line 4. |
| `zipcode`       | `string`          | No   | Postal code. |

### Success (200)
Returns the created address resource.

### Validation Error (422)
```json
{ "message": "The given data was invalid.", "errors": { "uuid": ["The uuid field must be a valid UUID."], "city": ["The city field is required."], "address_one": ["The address one field is required."] } }
```

### Unauthorized (401)
```json
{ "message": "Unauthenticated." }
```

### Forbidden (403)
Only for `POST /addresses` when the authenticated user lacks `backoffice` ability.
```json
{ "message": "This action is unauthorized." }
```

---

## Notes
- City resolution via pipeline accepts descriptive input or IDs.
- When `type = both`, response includes `main: true`; otherwise, billing/shipping/fiscal flags.
- Localisation: city/state/country names may follow `Accept-Language`.

