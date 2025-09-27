# ReputationBook – Complaint-Book Users (Update Role)

## Endpoint

```
PUT /api/v1/complaint-book/users/{user}/roles/{role}
```

Updates or creates the role assignment for the user in the current platform and optionally changes its status.

---

## Authentication

Required – `Bearer {token}` with abilities `backoffice` and `domain:reputationbook`.

---

## Headers

| Header | Type | Required | Description |
| ------ | ---- | -------- | ----------- |
| `Authorization` | string | Yes | Valid `Bearer {token}`. |
| `X-PUBLIC-KEY` | string | Yes | Public identifier for the platform. |
| `Accept-Language` | string | Recommended | Preferred locale for translatable fields. |

---

## Parameters

### Path

| Parameter | Type | Required | Description |
| --------- | ---- | -------- | ----------- |
| `user` | uuid | Yes | User UUID. |
| `role` | uuid \| integer \| string | Yes | Role identifier (UUID, internal ID, or canonical name). |

### Body (JSON)

| Field | Type | Required | Description |
| ----- | ---- | -------- | ----------- |
| `status` | string | No | New assignment status (`approved`, `disapproved`, `requested`). |

---

## Notes

- `PUT` behaves as an upsert: it creates the user↔role relation when missing and only updates the status when it already exists.
- Idempotent: repeating the same payload keeps the resulting state unchanged.

