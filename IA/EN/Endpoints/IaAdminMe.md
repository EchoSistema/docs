<!-- markdownlint-disable MD013 -->

# IA – Admin Profile Endpoint

## Endpoint

`GET /api/v1/ia/admin/me`

Returns the authenticated user's profile for the current platform.

---

## Authentication

Bearer token via Sanctum.

---

## Request

### Required Headers

| Header | Type | Description |
| ------ | ---- | ----------- |
| **Authorization** | `string` | Bearer {token} credential. |
| **X-PUBLIC-KEY** | `string` | Public key identifying the platform. |
| **Accept-Language** | `string` | Locale for translatable fields. Optional. |

No query parameters are supported.

---

## Example Response

```json
{
  "id": 123,
  "uuid": "u1v2w3x4-y5z6-7890-1234-56789abcdef0",
  "name": "Jane Doe",
  "gender": "female",
  "birth_date": "1990-05-12",
  "email": "jane@example.com",
  "roles": ["admin"],
  "permissions": ["store.company"],
  "contacts": {
    "public_email": {
      "email": "jane.public@example.com",
      "type": "public_email"
    },
    "whatsapp": {
      "ddi": "+55",
      "ddd": "11",
      "number": "999999999",
      "full": "+55 11 99999-9999",
      "type": "whatsapp"
    }
  },
  "platform": {"id": 1, "name": "PlatformName"},
  "images": {
    "avatar": {
      "uuid": "img-uuid",
      "unique_id": "a1b2c3",
      "usage": "avatar",
      "url": "https://cdn.example.com/avatar.jpg",
      "name": "Avatar",
      "slug": "avatar",
      "width": 512,
      "height": 512,
      "creation_date": "2025-01-01T12:00:00Z"
    }
  }
}
```

---

## JSON Structure Explanation

| Field | Type | Description |
| ----- | ---- | ----------- |
| `id` | `integer` | User ID. |
| `uuid` | `uuid` | User identifier. |
| `name` | `string` | Full name. |
| `gender` | `string` | Gender. |
| `birth_date` | `date` | Birth date. |
| `email` | `string` | Contact email. |
| `roles[]` | `array` | Assigned roles within the platform. |
| `permissions[]` | `array` | Granted permissions. |
| `contacts` | `object` | Public contact information keyed by type. |
| `platform` | `object` | Platform summary (id, name). |
| `images` | `object` | Uploaded images keyed by usage. |

---

## Notes

* `401 Unauthorized` for missing or invalid token.
* `403 Forbidden` when the user lacks a non-guest role on the platform.

<!-- markdownlint-enable MD013 -->
