# AI – Admin Users (Update)

## Endpoint

`PUT /api/v1/ia/admin/users/{user:uuid}`

Updates a user's profile fields: `name`, `email`, `gender`, `birth_date`, and `password`.

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

### Body (JSON)

At least one of the fields below must be present.

| Field | Type | Required | Description |
| ----- | ---- | -------- | ----------- |
| `name` | `string` | No | Full name. Max 255 chars. |
| `email` | `string` | No | Unique email in users table; validated with `email` and `email:dns`. |
| `gender` | `string` | No | One of: `m`, `f`, `o`. |
| `birth_date` | `date` | No | ISO-8601 date (e.g., `1992-03-10`). |
| `password` | `string` | No | Min 8 chars. Must be sent with `password_confirmation`. |
| `password_confirmation` | `string` | No | Must match `password` when provided. |

---

## Example Request

```bash
curl -X PUT \
  "/api/v1/ia/admin/users/9b732d6a-2e56-4db8-9b2e-6fd8c14e7f28" \
  -H "Authorization: Bearer {token}" \
  -H "X-PUBLIC-KEY: {public_key}" \
  -H "Accept-Language: en" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Jane Doe",
    "email": "jane.doe@example.com",
    "gender": "f",
    "birth_date": "1992-03-10",
    "password": "new-strong-pass",
    "password_confirmation": "new-strong-pass"
  }'
```

---

## Example Response

Same structure as “Admin Users (Show)”. Short example:

```json
{
  "uuid": "9b732d6a-2e56-4db8-9b2e-6fd8c14e7f28",
  "name": "Jane Doe",
  "email": "jane.doe@example.com",
  "avatar": null,
  "telephone": "+1 202 555 0147",
  "gender": "female",
  "birthday": "1992-03-10T00:00:00+00:00",
  "address": null,
  "nationalities": []
}
```

---

## HTTP Status

- 200: OK
- 400: Bad Request
- 401: Unauthorized
- 403: Forbidden
- 404: Not Found
- 422: Validation Error

---

## Notes

- Role updates are not allowed here. Use “Admin Users (Update Role)”.
- `password` is hashed automatically when updated.
- `email` uniqueness excludes the target user itself.

---

## Related

- docs/EN/IA/Endpoints/PlatformUserShow.md
- docs/EN/IA/Endpoints/PlatformUserUpdateRole.md

