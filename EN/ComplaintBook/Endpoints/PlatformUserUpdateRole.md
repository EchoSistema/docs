# Complaint Book – Users (Update Role)

## Endpoint

`PUT /api/v1/complaint-book/users/{user}/roles/{role}`

Updates the user's role for the current platform and/or the role status.

---

## Authentication

Required – Bearer token with `backoffice` ability and `domain:reputationbook`.

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

## Notes

- Uses PUT and behaves as upsert: creates the user↔role link when absent; updates status when present. Idempotent for the same payload.
