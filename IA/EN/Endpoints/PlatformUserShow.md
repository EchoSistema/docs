# Platform Users — Show

- Method: `GET`
- Route: `/api/v1/ia/admin/users/{user:uuid}`
- Auth: `auth:sanctum`

## Summary
Returns the user profile. Optionally includes the user’s roles within the current platform.

## Query Params
- `role` (bool) — when true, loads `platformRoles` limited to the current platform, ordered by `role_id`.

## Response
`UserProfileResource` fields:

```json
{
  "uuid": "...",
  "name": "...",
  "email": "...",
  "avatar": "...",
  "roles": [
    { "id": 5, "uuid": "...", "name": "guest", "localized_name": "Guest", "permissions": ["..."] }
  ],
  "telephone": "+1 ...",
  "gender": "...",
  "birthday": "2021-01-02T00:00:00Z",
  "address": { "city": "...", "state": "...", "country": { "name": "...", "code": "...", "flag": "..." } },
  "nationalities": [ { "uuid": "...", "country": { "name": "...", "code": "...", "flag": "..." } } ]
}
```

Notes:
- `roles` is included only when `role=true`.
- Address and nationalities are included when present on the user.

