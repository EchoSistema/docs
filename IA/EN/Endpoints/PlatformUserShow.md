# AI – Admin Users (Show)

## Endpoint

`GET /api/v1/ia/admin/users/{user:uuid}`

Returns a detailed user profile for the platform.

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

## Parameters

### Path

| Parameter | Type | Required | Description |
| --------- | ---- | -------- | ----------- |
| `user` | `uuid` | Yes | User UUID. |

### Query

| Parameter | Type | Required | Description |
| --------- | ---- | -------- | ----------- |
| `role` | `boolean` | No | When `true`, includes the user's roles for the current platform. |

---

## Example Response

```json
{
  "uuid": "9b732d6a-2e56-4db8-9b2e-6fd8c14e7f28",
  "name": "John Doe",
  "email": "john.doe@example.com",
  "avatar": "https://cdn.example.com/avatars/johndoe.webp",
  "roles": [
    {
      "id": 2,
      "uuid": "role-uuid",
      "name": "admin",
      "localized_name": "Administrator",
      "permissions": ["store.company"]
    }
  ],
  "telephone": "+1 202 555 0147",
  "gender": "male",
  "birthday": "1990-05-15T00:00:00+00:00",
  "address": {
    "country": {"name": "United States", "code": "US", "flag": "🇺🇸"},
    "state": "CA",
    "city": "San Francisco",
    "zipcode": "94103"
  },
  "nationalities": [
    {
      "uuid": "natio-uuid",
      "country": {"name": "United States", "code": "US", "flag": "🇺🇸"}
    }
  ]
}
```

---

## Explained JSON Structure

| Field | Type | Description |
| ----- | ---- | ----------- |
| `uuid` | `uuid` | Unique user identifier. |
| `name` | `string` | Full name. |
| `email` | `string` | Email address. |
| `avatar` | `string` | Avatar URL when available. |
| `roles[]` | `array` | Present when `role=true`; user roles on the current platform. |
| `telephone` | `string` | Phone when loaded. |
| `gender` | `string` | Gender. |
| `birthday` | `string` | ISO-8601 birth date. |
| `address` | `object` | Address when loaded. |
| `nationalities[]` | `array` | Nationalities with country info. |

---

## HTTP Status

- 200: OK
- 401: Unauthorized
- 403: Forbidden
- 404: Not Found

---

## Notes

- Use `role=true` to include roles and permissions for the current platform.
- Honors `Accept-Language` for translatable fields.

---

## Related

- docs/IA/EN/Endpoints/PlatformUserIndex.md
- docs/IA/EN/Endpoints/PlatformUserStore.md

