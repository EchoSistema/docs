# AI – Admin Users (Index)

## Endpoint

`GET /api/v1/ia/admin/users`

Lists platform users with filters by role, occupation, and area for the AI admin area.

---

## Authentication

Required – Bearer token with `backoffice` ability.

---

## Headers

| Header | Type | Required | Description |
| ------ | ---- | -------- | ----------- |
| Authorization | `string` | Yes | `Bearer {token}`. |
| X-PUBLIC-KEY | `string` | Yes | Platform public key. |
| Accept-Language | `string` | Recommended | Language for translatable fields (e.g., `pt-BR`, `en`, `es`). |

---

## Query Parameters

Accepts the same filters documented in Shared Platform User Index. See:

- docs/EN/Shared/Endpoints/PlatformUserIndex.md

Supported parameters include (not exhaustive): `role`, `roles[]`, `role_id`, `role_name`, `user_name`, `user_email`, `user_uuid`, `job_occupation` (and variations), `occupation_area` (and variations), `has_job_occupation`, `has_occupation_area`, `no_paginate`, `per_page`.

> Parameters may be sent in canonical `snake_case` and `camelCase`/`kebab-case` variants.

---

## Response

Identical to the Shared Platform User Index response. Example and detailed structure:

- docs/EN/Shared/Endpoints/PlatformUserIndex.md

---

## HTTP Status

- 200: OK
- 401: Unauthorized
- 403: Forbidden
- 422: Unprocessable Entity

---

## Notes

- Returns only users with roles lower than the authenticated user's minimum role on the current platform.
- When `has_job_occupation` or `has_occupation_area` are true, titles are loaded based on `Accept-Language`.
- With `no_paginate=true`, returns all results without pagination metadata.

---

## Related

- docs/EN/IA/Endpoints/PlatformUserCounter.md
- docs/EN/IA/Endpoints/PlatformUserShow.md
- docs/EN/IA/Endpoints/PlatformUserStore.md

- docs/PT/IA/Endpoints/PlatformUserIndex.md
- docs/ES/IA/Endpoints/PlatformUserIndex.md
