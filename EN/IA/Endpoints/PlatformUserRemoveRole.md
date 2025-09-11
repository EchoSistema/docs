# Platform Users — Remove Role

- Method: `DELETE`
- Route: `/api/v1/ia/admin/users/{user:uuid}/roles/{role}`
- Auth: `auth:sanctum`

## Summary
Removes a role from the user within the current platform. If it is the only role the user has, it is replaced by `GUEST` with `status=approved` (the user is never left without a role).

## Path Params
- `user` — uuid
- `role` — accepts `id`, `uuid`, or `name`

## Response
`UserProfileResource` with `platformRoles` loaded.

## Notes
- No-op if the user does not hold the specified role in this platform.

