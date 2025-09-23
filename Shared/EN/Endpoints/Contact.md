# Contacts — Create/Update (IA Admin)

## Endpoints

- `POST /ia/admin/contacts`
- `PUT /ia/admin/contacts/{contact}`

Creates or updates a user contact (email or phone-based: telephone, whatsapp, telegram).

---

## Authentication

Required — `auth:sanctum`.

### Required Headers
| Header | Type | Description |
| ------ | ---- | ----------- |
| Authorization | `string` | `Bearer <token>` required. |
| Accept-Language | `string` | Locale for translatable fields (e.g., `en`, `pt-BR`). Optional. |

---

## POST /ia/admin/contacts

Creates a contact attached to a user by UUID.

### Body (JSON)
| Field          | Type                       | Req. | Description |
| -------------- | -------------------------- | ---- | ----------- |
| `uuid`         | `uuid`                     | Yes  | User UUID to attach the contact to. |
| `type`         | `ContactTypeEnum`          | Yes  | One of `email`, `telephone`, `whatsapp`, `telegram`. |
| `email`        | `email`                    | Cond | Required if `type = email` (or use `value`). |
| `value`        | `email`                    | Cond | Alternative to `email` when `type = email`. |
| `country_code` | `string`                   | Cond | Required with `number` when `type` is not `email`. Accepts `+` and digits (other chars ignored). |
| `number`       | `string`                   | Cond | Phone number digits when `type` is not `email` (non-digits ignored). |

Validation requires either email fields (for `email` type) or phone fields (for other types).

### Request Examples
Email contact:
```json
{
  "uuid": "11111111-1111-1111-1111-111111111111",
  "type": "email",
  "email": "john.doe@example.com"
}
```

Phone contact (WhatsApp):
```json
{
  "uuid": "11111111-1111-1111-1111-111111111111",
  "type": "whatsapp",
  "country_code": "+55",
  "number": "11 91234-5678"
}
```

### Responses
- `200 OK` with the created contact resource.

### Response Examples
Email:
```json
{ "email": "john.doe@example.com" }
```

WhatsApp/Telephone/Telegram:
```json
{
  "whatsapp": {
    "country_code": "+55",
    "number": "11912345678",
    "full": "+55 11912345678"
  }
}
```

### Validation Error (422)
Example when required fields are missing or invalid for the chosen `type`.
```json
{
  "message": "The given data was invalid.",
  "errors": {
    "uuid": ["The uuid field must be a valid UUID."],
    "email": ["The email field is required when type is email."],
    "country_code": ["The country code field is required when type is not email."],
    "number": ["The number field is required when type is not email."]
  }
}
```

### Not Found (404)
When the provided user UUID does not match any user.
```json
{
  "message": "User not found."
}
```

### Unauthorized (401)
When the request lacks a valid Sanctum token.
```json
{ "message": "Unauthenticated." }
```

### Forbidden (403)
When the authenticated user does not have permission for this action.
```json
{ "message": "This action is unauthorized." }
```

---

## PUT /ia/admin/contacts/{contact}

Updates an existing contact. Fields are optional and only the provided ones are updated. When switching `type`, the stored structure adapts accordingly.

### Body (JSON)
| Field          | Type              | Req. | Description |
| -------------- | ----------------- | ---- | ----------- |
| `type`         | `ContactTypeEnum` | No   | Change contact type (`email`, `telephone`, `whatsapp`, `telegram`). |
| `email`        | `email`           | No   | Email (for `email` type) or use `value`. |
| `value`        | `email`           | No   | Alternative to `email` for `email` type. |
| `country_code` | `string`          | No   | `+` and digits; non-digits ignored. |
| `number`       | `string`          | No   | Phone digits; non-digits ignored. |
| `uuid`         | —                 | —    | Prohibited (cannot change related user). |

### Responses
- `200 OK` with the updated contact resource.

### Validation Error (422)
Example when sending invalid fields or trying to change a prohibited attribute.
```json
{
  "message": "The given data was invalid.",
  "errors": {
    "email": ["The email must be a valid email address."],
    "uuid": ["The uuid field is prohibited."]
  }
}
```

### Not Found (404)
When the contact does not exist.
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
- For phone-based contacts, `country_code` and `number` are sanitized to digits, and `country_code` is stored with a leading `+`.
- Returned shape depends on `type`: either `{ "email": "..." }` or `{ "<type>": { country_code, number, full } }`.
- Localisation: if any translatable fields are present in related data, their values follow the `Accept-Language` header when available; otherwise, defaults are returned.
