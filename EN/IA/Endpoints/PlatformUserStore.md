# Platform Users — Store (Register)

- Method: `POST`
- Route: `/api/v1/ia/admin/users`
- Auth: `auth:sanctum`

## Summary
Registers a new user on the current platform using the registration pipeline. Returns the user data and, by default in this flow, no token.

## Request Body
Required:
- `device` (string)
- `name` (string)
- `email` (email)
- `password` (string, min 8)
- `password_confirmation` (string)

Optional (common):
- `language` (enum)
- `currency` (enum)

Optional (when `collaborator=true`):
- `collaborator` (bool)
- `gender` (enum)
- `birth_date` (date)
- `roles[]` (array of names; e.g. `guest`, `support`)
- `address{...}` (see validation rules)
- `contacts[]` with `type`/`value`/`country_code`/`number`
- `nationalities[]`

Notes:
- The controller injects flags: `recently_created=true`, `no_auth=true`, `registered_by=true`.
- When `roles` includes `guest`, `required` fields for address are relaxed.

## Response
`UserAuthResource` shape (token omitted due to `no_auth=true`):

```json
{
  "message": "User successfully registered.",
  "recently_created": true,
  "data": {
    "user": {
      "uuid": "...",
      "echo_uuid": "...",
      "name": "...",
      "email": "...",
      "avatar": "...",
      "language": "en",
      "roles": [
        {
          "id": 5,
          "platform": { "uuid": "...", "name": "...", "public_key": "..." },
          "name": "guest",
          "localized_name": "Guest",
          "permissions": [ { "subject": "users", "action": "read" } ]
        }
      ],
      "registered_by": { }
    }
  }
}
```

