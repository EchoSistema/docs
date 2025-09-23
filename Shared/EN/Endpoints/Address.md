# Addresses — Create/Update

## Endpoints

- `POST /addresses` (Backoffice)
- `POST /ia/admin/addresses` (IA Admin)
- `PUT /ia/admin/addresses/{address}` (IA Admin)

Creates or updates addresses. Both endpoints share the same payload rules; `PUT` updates an existing resource.

---

## Authentication

- `POST /addresses`: `auth:sanctum` with `ability:backoffice`.
- `POST|PUT /ia/admin/addresses`: `auth:sanctum`.

### Required Headers
| Header | Type | Description |
| ------ | ---- | ----------- |
| Authorization | `string` | `Bearer <token>` required. |
| Accept-Language | `string` | Locale for translatable fields (e.g., `en`, `pt-BR`). Optional. |

---

## POST /addresses and POST /ia/admin/addresses

Creates an address. When using these routes without nested resources, `uuid` is required to bind the address to an entity.

### Body (JSON)
| Field           | Type              | Req. | Description |
| --------------- | ----------------- | ---- | ----------- |
| `type`          | `AddressTypeEnum` | No   | Defaults to `both`. One of `billing`, `shipping`, `both`, `fiscal`, `additional`. |
| `uuid`          | `uuid`            | Cond | Required when not using nested `addressable` routes. |
| `city`          | `string|object`   | Yes  | City reference; resolved via pipeline. May be a name or an object. |
| `address_one`   | `string`          | Yes  | Address line 1. |
| `address_two`   | `string`          | No   | Address line 2. |
| `address_three` | `string`          | No   | Address line 3. |
| `address_four`  | `string`          | No   | Address line 4. |
| `zipcode`       | `string`          | No   | Postal code. |

### Responses
- `200 OK` with the created address resource.

### Response Example
```json
{
  "uuid": "aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa",
  "main": true,
  "zipcode": "01000-000",
  "one": "Av. Paulista, 1000",
  "two": null,
  "three": null,
  "four": null,
  "city": { "uuid": "...", "name": "São Paulo", "abbreviation": "SP" },
  "state": { "uuid": "...", "name": "São Paulo", "abbreviation": "SP", "region": "Southeast" },
  "country": { "uuid": "...", "name": "Brazil", "official_name": "Federative Republic of Brazil", "code": "BR" },
  "formatted": "Av. Paulista, 1000 - São Paulo/SP, 01000-000, Brazil"
}
```

### Validation Error (422)
Example when required fields are missing or invalid.
```json
{
  "message": "The given data was invalid.",
  "errors": {
    "uuid": ["The uuid field must be a valid UUID."],
    "city": ["The city field is required."],
    "address_one": ["The address one field is required."]
  }
}
```

### Unauthorized (401)
When the request lacks a valid Sanctum token.
```json
{ "message": "Unauthenticated." }
```

### Forbidden (403)
Only for `POST /addresses` when the authenticated user lacks `backoffice` ability.
```json
{ "message": "This action is unauthorized." }
```

If `type` is not `both`, the response includes flags:
```json
{
  "uuid": "...",
  "main": false,
  "is_billing": true,
  "is_shipping": false,
  "is_fiscal": false,
  "zipcode": "..."
}
```

---

## PUT /ia/admin/addresses/{address}

Updates an existing address. Only provided fields are changed; `uuid` changes are not allowed.

### Body (JSON)
| Field           | Type              | Req. | Description |
| --------------- | ----------------- | ---- | ----------- |
| `type`          | `AddressTypeEnum` | No   | Address type. |
| `city`          | `string|object`   | No   | City reference; can also use IDs below. |
| `city_id`       | `integer`         | No   | City ID. |
| `state_id`      | `integer`         | No   | State ID. |
| `country_id`    | `integer`         | No   | Country ID. |
| `zipcode`       | `string`          | No   | Postal code. |
| `address_one`   | `string`          | No   | Address line 1. |
| `address_two`   | `string`          | No   | Address line 2. |
| `address_three` | `string`          | No   | Address line 3. |
| `address_four`  | `string`          | No   | Address line 4. |
| `uuid`          | —                 | —    | Prohibited (cannot change address relationship). |

### Responses
- `200 OK` with the updated address resource.

### Validation Error (422)
Example when sending invalid fields or trying to modify prohibited attributes.
```json
{
  "message": "The given data was invalid.",
  "errors": {
    "uuid": ["The uuid field is prohibited."],
    "city_id": ["The city id must be an integer."],
    "zipcode": ["The zipcode must be a string."]
  }
}
```

### Not Found (404)
When the address does not exist.
```json
{ "message": "Not Found" }
```

### Unauthorized (401)
Missing or invalid token.
```json
{ "message": "Unauthenticated." }
```

### Forbidden (403)
Authenticated but not allowed to update this resource.
```json
{ "message": "This action is unauthorized." }
```

---

## Notes
- City resolution is handled by pipeline; either a descriptive input or direct IDs may be used.
- When `type = both`, the resource marks `main: true`; otherwise, specific flags indicate billing/shipping/fiscal roles.
- Localisation: city, state and country names may be localised according to the `Accept-Language` header when translations are available; otherwise default labels are returned.
