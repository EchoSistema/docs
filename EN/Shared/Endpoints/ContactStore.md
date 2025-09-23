# Contacts — Create (IA Admin)

## Endpoint

`POST /ia/admin/contacts`

Creates a user contact (email or phone-based: telephone, whatsapp, telegram).

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
| `uuid`         | `uuid`            | Yes  | User UUID to attach the contact to. |
| `type`         | `ContactTypeEnum` | Yes  | One of `email`, `telephone`, `whatsapp`, `telegram`. |
| `email`        | `email`           | Cond | Required if `type = email` (or use `value`). |
| `value`        | `email`           | Cond | Alternative to `email` when `type = email`. |
| `country_code` | `string`          | Cond | With `number` when `type` is not `email`. Accepts `+` and digits (others ignored). |
| `number`       | `string`          | Cond | Phone digits when `type` is not `email` (non-digits ignored). |

Validation requires either email fields (for `email` type) or phone fields (for other types).

### Request Examples
Email contact:
```json
{ "uuid": "11111111-1111-1111-1111-111111111111", "type": "email", "email": "john.doe@example.com" }
```

Phone contact (WhatsApp):
```json
{ "uuid": "11111111-1111-1111-1111-111111111111", "type": "whatsapp", "country_code": "+55", "number": "11 91234-5678" }
```

### Success (200)
Email:
```json
{ "email": "john.doe@example.com" }
```
WhatsApp/Telephone/Telegram:
```json
{ "whatsapp": { "country_code": "+55", "number": "11912345678", "full": "+55 11912345678" } }
```

### Validation Error (422)
```json
{ "message": "The given data was invalid.", "errors": { "uuid": ["The uuid field must be a valid UUID."], "email": ["The email field is required when type is email."], "country_code": ["The country code field is required when type is not email."], "number": ["The number field is required when type is not email."] } }
```

### Not Found (404)
When the provided user UUID does not match any user.
```json
{ "message": "User not found." }
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
- For phone-based contacts, `country_code` and `number` are sanitized; `country_code` is stored with a leading `+`.
- Returned shape depends on `type`: `{ "email": "..." }` or `{ "<type>": { country_code, number, full } }`.
- Localisation: translatable fields follow `Accept-Language` when available.

