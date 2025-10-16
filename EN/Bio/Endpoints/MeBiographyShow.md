# Bio – My Biography Show

## Endpoint

```
GET /api/v1/bio/me
```

## Authentication

Required – Bearer {token} with ability `backoffice` and permission `domain:bio`.

## Headers

| Header           | Type   | Required | Description |
| ---------------- | ------ | -------- | ----------- |
| Authorization    | string | Yes      | `Bearer {token}`. |
| X-PUBLIC-KEY     | string | Yes      | Platform public key. |
| Accept-Language  | string | No       | IETF locale (e.g., `pt-BR`, `en`, `es`). |

## Parameters

None.

## Examples

### Request example (curl)

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: en" \
  "https://sandbox.your-domain.com/api/v1/bio/me"
```

### Response example

```json
{
  "data": {
    "id": 1,
    "uuid": "550e8400-e29b-41d4-a716-446655440000",
    "user_id": 123,
    "slug": "john-doe",
    "public_email": "john@example.com",
    "summary": "Software engineer with 10 years of experience",
    "headline": "Senior Software Engineer",
    "created_at": "2025-01-15T10:30:00.000000Z",
    "updated_at": "2025-01-15T10:30:00.000000Z",
    "user": {
      "id": 123,
      "name": "John Doe",
      "email": "john@example.com"
    }
  }
}
```

## JSON Structure Explained

| Field           | Type    | Description |
| --------------- | ------- | ----------- |
| data            | object  | Biography object. |
| data.id         | integer | Biography identifier. |
| data.uuid       | string  | Unique biography identifier. |
| data.user_id    | integer | Associated user ID. |
| data.slug       | string  | URL-friendly slug. |
| data.public_email| string | Public email address. |
| data.summary    | string  | Biography summary. |
| data.headline   | string  | Professional headline. |
| data.user       | object  | Associated user object. |

## HTTP Status

- 200: OK
- 401: Unauthorized
- 403: Forbidden
- 500: Internal Server Error

## Errors

```json
{
  "message": "Unauthenticated."
}
```

## Notes

- Returns the biography of the authenticated user.
- If no biography exists, a default biography object is created and returned.

## Related

- [My Biography — Update](MeBiographyUpdate.md)
