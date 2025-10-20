# Shared – Update User (Admin)

## Endpoint

`PUT /api/v1/users/{user:uuid}`

## Authentication

Requires Bearer token with `backoffice` ability and platform permissions.

Permissions
- One of: `update.guest`, `update.collaborator`, or `update.all` on the current platform role.

## Headers

| Header | Type | Required | Description |
| ------ | ---- | -------- | ----------- |
| Authorization | `string` | Yes | `Bearer <token>`. |
| X-PUBLIC-KEY  | `string` | Yes | Platform public key. |
| Accept-Language | `string` | No | Locale for validation messages. |

## Path Parameters

| Param | Type | Required | Description |
| ----- | ---- | -------- | ----------- |
| `user` | `uuid` | Yes | Target user identifier. |

## Body (application/json)

| Field | Type | Required | Description |
| ----- | ---- | -------- | ----------- |
| `name` | `string` | No | Full name (max 255). |
| `gender` | `string` | No | Accepts `m`/`f`/`o` or normalized values `male`/`female`/`other`. |
| `birth_date` | `date` | No | Birth date (ISO 8601). |
| `email` | `email` | No | Email address (max 255). |

Notes
- The server normalizes `gender`: `male`→`m`, `female`→`f`, others→`o`.
- Only the provided fields are updated; unspecified fields remain unchanged.

## Example Request

```json
{
  "name": "Jane Admin-Updated",
  "gender": "female",
  "birth_date": "1988-09-20",
  "email": "jane.admin@example.com"
}
```

## Success `200 OK`

Returns the full user profile resource (same shape as “Get User Profile”).

```json
{
  "data": {
    "uuid": "11111111-2222-3333-4444-555555555555",
    "echo_uuid": "encoded_uuid",
    "name": "Jane Admin-Updated",
    "email": "jane.admin@example.com",
    "email_verified_at": null,
    "slug": "jane-admin",
    "avatar": {"url": "https://cdn.example.com/users/jane/avatar.webp", "usage": "avatar"},
    "profile_image": null,
    "age": 36,
    "gender": "f",
    "gender_name": "female",
    "birthday": "1988-09-20T00:00:00+00:00",
    "is_banned": false,
    "is_foreign": false,
    "is_master": false,
    "language": "en",
    "currency": "USD",
    "created_at": "2024-02-10T12:00:00+00:00",
    "roles": [
      {"id": 2, "uuid": "role-admin-uuid", "name": "administrator", "localized_name": "Administrator", "permissions": ["index.platform_message", "show.platform_message", "update.all"]}
    ],
    "telephone": "+1 555-0200",
    "contacts": {"email": {"uuid": "c1", "name": "email", "email": "jane.admin@example.com", "phone": null}},
    "social_medias": {"linkedin": {"uuid": "s1", "name": "linkedin", "url": "https://linkedin.com/in/jane"}},
    "address": {"uuid": "addr-uuid", "main": true, "zipcode": "10001", "one": "5th Avenue", "formatted": "5th Avenue, New York - NY, 10001, US"},
    "nationalities": [{"id": 840, "country_id": 840, "country": {"name": "United States", "code": "US", "flag": "🇺🇸"}}],
    "biography": {"uuid": "bio-uuid", "name": "Jane Admin-Updated", "summary": "..."},
    "platform": {"id": 1, "uuid": "plat-uuid", "name": "Echosistema"},
    "affiliate": null,
    "identities": []
  }
}
```

## JSON Structure (highlights)

Same as “Update My Profile”. See also “Get User Profile” for full schema.

## Errors

- 401 Unauthorized — missing/invalid token
- 403 Forbidden — insufficient permissions or platform mismatch
- 404 Not Found — user not found
- 422 Unprocessable Entity — validation errors
- 500 Internal Server Error

### 422 Example

```json
{
  "message": "The given data was invalid.",
  "errors": {
    "email": ["The email must be a valid email address."],
    "gender": ["The selected gender is invalid."],
    "birth_date": ["The birth date is not a valid date."]
  }
}
```

## Related

- [Get User Profile](./UserProfile.md)
- [Update My Profile](./UserProfileUpdate.md)
