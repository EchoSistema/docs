# Platform Users — Index

- Method: `GET`
- Route: `/api/v1/ia/admin/users`
- Auth: `auth:sanctum`

## Summary
Lists platform users for the current platform, returning platform-role records with the related user. Supports rich filtering and optional pagination.

## Query Params
- Pagination: `page` (int), `per_page` (int, default 25), `no_paginate` (bool)
- Relations: `relations[]` — default includes `user`, `user.avatarImage`, `user.telephone`
- Localization: `language` — used when requesting occupation relations
- Role filters:
  - `role` (id or name)
  - `roles[]` (array of ids or names)
  - or explicit: `role_id`, `role_name`, `role_ids[]`, `role_names[]`
- User filters: `name` or `user_name` (contains), `email` or `user_email` (contains), `user_uuid`
- Occupation filters:
  - `job_occupation` (id | uuid | title) → normalized into `job_occupation_id` | `job_occupation_uuid` | `job_occupation_title`
  - `occupation_area` (uuid | `content:usage`) or structured `occupation_area[content]`, `occupation_area[usage]`
  - Convenience flags (auto-derived): `has_job_occupation`, `has_occupation_area`

## Behavior
- Excludes roles below the current operator’s minimum role in the platform.
- When `has_job_occupation` or `has_occupation_area` are true, it eagerly loads `user.jobOccupations` (default only) with localized titles according to `language`.

## Response
Returns a collection in the standard shape:

```json
{
  "data": [
    {
      "platform_id": 1,
      "user_id": 10,
      "role_id": 5,
      "status": "approved",
      "user": { "uuid": "...", "name": "...", "email": "...", "avatar": "..." }
    }
  ],
  "links": { "first": "...", "last": "...", "prev": null, "next": "..." },
  "meta": { "current_page": 1, "per_page": 25, "total": 123 }
}
```

Notes:
- `links`/`meta` appear when paginated; omitted when `no_paginate=true`.
- Each item is a `platform_user_roles` record possibly with requested relations.

## Examples
- Basic: `GET /api/v1/ia/admin/users?per_page=25`
- Filter by role name: `GET ...?role=guest`
- Filter by multiple role ids: `GET ...?roles[]=1&roles[]=2`
- Filter by user email contains: `GET ...?email=@domain.com`
- Include occupation and localized titles: `GET ...?job_occupation=Some Title&language=en`

