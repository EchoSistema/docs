# AI – Admin Users (Create)

## Endpoint

`POST /api/v1/ia/admin/users`

Creates a new user on the platform (AI admin area).

---

## Authentication

Required – Bearer token with `backoffice` ability.

---

## Headers

| Header | Type | Required | Description |
| ------ | ---- | -------- | ----------- |
| Authorization | `string` | Yes | `Bearer {token}`. |
| X-PUBLIC-KEY | `string` | Yes | Platform public key. |
| Accept-Language | `string` | Recommended | Language for translatable fields (e.g., `pt-BR`, `en`, `es`). |

---

## Request Body

Minimum required fields:

| Field | Type | Required | Description |
| ----- | ---- | -------- | ----------- |
| `device` | `string` | Yes | Device/client identifier. |
| `name` | `string` | Yes | User name. |
| `email` | `string` | Yes | Unique email within the platform. |
| `password` | `string` | Yes | Password (min. 8). |
| `password_confirmation` | `string` | Yes | Password confirmation. |

Additional fields (when `collaborator=true`, they become required together):

| Field | Type | Required | Description |
| ----- | ---- | -------- | ----------- |
| `collaborator` | `boolean` | No | Enables collaborator flow. |
| `gender` | `string` | Conditional | Gender (enum). |
| `birth_date` | `date` | Conditional | Birth date. |
| `language` | `string` | No | User language (enum). |
| `currency` | `string` | No | Currency (supported ISO code). |
| `roles[]` | `array<string>` | Conditional | Roles (valid names). |
| `nationalities[]` | `array` | Conditional | Nationalities. |
| `address` | `object` | Conditional | User address. |
| `contacts[]` | `array<object>` | Conditional | Contacts (email/phone/whatsapp, etc.). |

Normalization notes:

- `contacts[*].type` accepts name or `type`; it is normalized and `contactable` defaults to `user`.
- `address.addressable` defaults to `user`.

### Example Request

```json
{
  "device": "admin-panel",
  "name": "Jane Doe",
  "email": "jane@example.com",
  "password": "secret123",
  "password_confirmation": "secret123",
  "collaborator": true,
  "gender": "female",
  "birth_date": "1990-05-12",
  "roles": ["admin"],
  "address": {"country_id": 76},
  "contacts": [
    {"type": "public_email", "value": "jane.public@example.com"},
    {"type": "whatsapp", "country_code": "+55", "number": "11999999999"}
  ]
}
```

---

## Example Response

Note: this endpoint does not return a `token` (admin/internal usage); it includes `recently_created` and `registered_by`.

```json
{
  "message": "User Jane Doe created successfully.",
  "recently_created": true,
  "data": {
    "user": {
      "uuid": "u1v2w3x4-y5z6-7890-1234-56789abcdef0",
      "echo_uuid": "echo-uuid",
      "name": "Jane Doe",
      "email": "jane@example.com",
      "avatar": null,
      "language": "en",
      "roles": [
        {
          "id": 2,
          "platform": {"uuid": "plat-uuid", "name": "Platform", "public_key": "..."},
          "name": "admin",
          "localized_name": "Administrator",
          "permissions": [{"subject": "company", "action": "store"}]
        }
      ],
      "registered_by": {"uuid": "admin-uuid", "name": "Admin"}
    }
  }
}
```

---

## HTTP Status

- 200: OK (created)
- 401: Unauthorized
- 403: Forbidden
- 422: Unprocessable Entity

---

## Notes

- No `token` is returned as this is an admin/panel creation flow.
- The body is processed by a registration pipeline that normalizes contacts and address.

---

## Related

- docs/IA/EN/Endpoints/PlatformUserIndex.md
- docs/IA/EN/Endpoints/PlatformUserShow.md

