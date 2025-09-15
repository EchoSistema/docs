# Platform Users — Update Role

- Method: `PUT`
- Route: `/api/v1/ia/admin/users/{user:uuid}/roles/{role}`
- Auth: `auth:sanctum`

## Summary
Adds or updates a user role within the current platform.

## Path Params
- `user` — uuid
- `role` — accepts `id`, `uuid`, or `name`

## Request Body
- `status` (optional) — one of `approved`, `disapproved`, `requested`

## Behavior
- If the role does not exist for the user in this platform, it creates it (default `status=requested`).
- If already exists, updates the `status` (and ensures `role_id` matches the provided role).

## Response
`UserProfileResource` with `platformRoles` loaded.

