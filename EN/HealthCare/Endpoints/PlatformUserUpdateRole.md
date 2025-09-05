# Health-Care – Users (Update Role)

## Endpoint

`PUT /api/v1/health-care/users/{user}/roles/{role}`

Updates the user's role for the current platform and/or the role status.

---

## Authentication

Required – Bearer token with `backoffice` ability and `domain:healthcare`.

---

## Headers

| Header | Type | Required | Description |
| ------ | ---- | -------- | ----------- |
| Authorization | `string` | Yes | `Bearer {token}`. |
| X-PUBLIC-KEY | `string` | Yes | Platform public key. |
| Accept-Language | `string` | Recommended | Language for translatable fields. |

---

## Parameters

### Path

| Parameter | Type | Required | Description |
| --------- | ---- | -------- | ----------- |
| `user` | `uuid` | Yes | User UUID. |
| `role` | `uuid, int, string` | Yes | Role identifier (UUID, ID, or name). |

### Body (JSON)

| Field | Type | Required | Description |
| ----- | ---- | -------- | ----------- |
| `status` | `string` | No | One of: `approved`, `disapproved`, `requested`. |

---

## Example

```bash
curl -X PUT \
  "/api/v1/health-care/users/123/roles/admin" \
  -H "Authorization: Bearer {token}" \
  -H "X-PUBLIC-KEY: {public_key}" \
  -H "Accept-Language: en" \
  -H "Content-Type: application/json" \
  -d '{"status": "approved"}'
```

---

## HTTP Status

- 200, 401, 403, 404, 409, 422

---

## Notes

- Uses PUT and behaves as upsert: creates the user↔role link when absent; updates status when present. Idempotent for the same payload.
