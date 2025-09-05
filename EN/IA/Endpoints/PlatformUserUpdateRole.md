# AI – Admin Users (Update Role)

## Endpoint

`PUT /api/v1/ia/admin/users/{user:uuid}/roles/{role}`

Updates the user's role for the current platform and/or the role status.

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
| `role` | `string|uuid|int` | Yes | Role identifier (name, UUID, or ID). |

### Body (JSON)

At least one of `role_id`, `role_name`, or `status` must be present.

| Field | Type | Required | Description |
| ----- | ---- | -------- | ----------- |
| `status` | `string` | No | One of: `approved`, `disapproved`, `requested`. |

---

## Example Requests

Change by role name:

```bash
curl -X PUT \
  "/api/v1/ia/admin/users/9b732d6a-2e56-4db8-9b2e-6fd8c14e7f28/roles/admin" \
  -H "Authorization: Bearer {token}" \
  -H "X-PUBLIC-KEY: {public_key}" \
  -H "Accept-Language: en" \
  -H "Content-Type: application/json" \
  -d '{"status": "approved"}'
```

---

## Example Response

Returns the same profile structure as “Admin Users (Show)”, with roles loaded when applicable.

```json
{
  "uuid": "9b732d6a-2e56-4db8-9b2e-6fd8c14e7f28",
  "name": "Jane Doe",
  "email": "jane.doe@example.com",
  "avatar": null,
  "roles": [
    {
      "id": 2,
      "uuid": "role-uuid",
      "name": "admin",
      "localized_name": "Administrator",
      "permissions": ["store.company"]
    }
  ]
}
```

---

## HTTP Status

- 200: OK
- 400: Bad Request
- 401: Unauthorized
- 403: Forbidden
- 404: Not Found (e.g., no platform role to update)
- 409: Conflict (duplicate user role on platform)
- 422: Validation Error

---

## Notes

- Role updates are restricted to the current platform.
- Uses PUT and behaves as upsert: creates the user↔role link when absent; updates status when present. Idempotent for the same payload.
