# ReputationBook – Collaborator Profile

## Endpoint

```
GET /api/v1/complaint-book/collaborators/me
```

Returns information about the authenticated collaborator within the platform, including roles assigned.

## Authentication

Required – `Bearer {token}` (`auth:sanctum`) with abilities `backoffice` and `domain:reputationbook`.

## Response

Follows `CollaboratorResource`:

- Personal data (`id`, `uuid`, `name`, `email`, `gender`, `birth_date`).
- `roles`: list of roles attached to the current platform (localized name, permissions grouped by `subject`/`action`).
- `contacts`: map with public contacts (`public_email`, `telephone`, `whatsapp`, etc.).
- `platform`: basic info about the platform (id, name).
- `avatar` when present and any extra images (`usage`, `url`, dimensions).

## HTTP Status Codes

- 200: Profile returned.
- 401: Invalid token.
- 403: User does not hold a role for the platform (`auth.forbidden_platform`).

