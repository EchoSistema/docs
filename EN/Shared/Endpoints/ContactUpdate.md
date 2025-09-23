# Contacts — Update (IA Admin)

## Endpoint

`PUT /ia/admin/contacts/{contact}`

Updates an existing user contact.

---

## Authentication

Required — `auth:sanctum`.

### Required Headers
| Header | Type | Description |
| ------ | ---- | ----------- |
| Authorization | `string` | `Bearer <token>` required. |
| Accept-Language | `string` | Locale for translatable fields (e.g., `en`, `pt-BR`). Optional. |

---

## Body (JSON)
| Field          | Type              | Req. | Description |
| -------------- | ----------------- | ---- | ----------- |
| `type`         | `ContactTypeEnum` | No   | Change type: `email`, `telephone`, `whatsapp`, `telegram`. |
| `email`        | `email`           | No   | Email (for `email` type) or use `value`. |
| `value`        | `email`           | No   | Alternative to `email` for `email` type. |
| `country_code` | `string`          | No   | `+` and digits; non-digits ignored. |
| `number`       | `string`          | No   | Digits only; non-digits ignored. |
| `uuid`         | —                 | —    | Prohibited (cannot change related user). |

### Success (200)
Returns the updated contact resource in the same shape as create.

### Validation Error (422)
```json
{ "message": "The given data was invalid.", "errors": { "email": ["The email must be a valid email address."], "uuid": ["The uuid field is prohibited."] } }
```

### Not Found (404)
When the contact does not exist.
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
- Localisation: translatable fields follow `Accept-Language` when available.

