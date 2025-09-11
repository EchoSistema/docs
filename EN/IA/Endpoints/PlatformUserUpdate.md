# Platform Users — Update

- Method: `PUT`
- Route: `/api/v1/ia/admin/users/{user:uuid}`
- Auth: `auth:sanctum`

## Summary
Updates basic user fields.

## Request Body (any of)
- `name` (string)
- `email` (email, unique)
- `gender` (enum)
- `birth_date` (date)
- `password` + `password_confirmation` (string, min 8)

## Response
`UserProfileResource` — same shape as Show.

