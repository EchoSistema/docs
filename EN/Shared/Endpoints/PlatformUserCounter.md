# Artificial Intelligence – Admin Users Counter

## Endpoint

`GET /api/v1/ia/admin/users/counter`

Returns the total number of platform users per role that match the applied filters in the Artificial Intelligence admin area. Behavior and filters match the Shared Users Counter endpoint.

See also: docs/EN/IA/Endpoints/PlatformUserCounter.md

---

## Authentication

Required – Bearer token with `backoffice` ability.

---

## Query Parameters

Accepts the same filter parameters as the AI Users Index (and the Shared Users Index). Refer to:

- docs/EN/IA/Endpoints/PlatformUserIndex.md
- docs/EN/Shared/Endpoints/PlatformUserIndex.md

---

## Response

Identical to the Shared Users Counter response. Example:

```json
{
  "counter": {
    "admin": {
      "role_id": 2,
      "name": "admin",
      "localized_name": "Administrator",
      "total": 10
    }
  }
}
```

For more details, see:

- docs/EN/IA/Endpoints/PlatformUserCounter.md
