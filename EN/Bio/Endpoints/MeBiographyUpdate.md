# Bio – My Biography Update

## Endpoint

```
PUT /api/v1/bio/me
```

## Authentication

Required – Bearer {token} with ability `backoffice` and permission `domain:bio`.

## Headers

| Header           | Type   | Required | Description |
| ---------------- | ------ | -------- | ----------- |
| Authorization    | string | Yes      | `Bearer {token}`. |
| X-PUBLIC-KEY     | string | Yes      | Platform public key. |
| Accept-Language  | string | No       | IETF locale (e.g., `pt-BR`, `en`, `es`). |
| Content-Type     | string | Yes      | `application/json`. |

## Parameters

### Request body

```json
{
  "slug": "john-doe-developer",
  "public_email": "john.public@example.com",
  "summary": "Experienced software engineer specializing in web development",
  "headline": "Senior Full-Stack Engineer"
}
```

| Field        | Type   | Required | Description |
| ------------ | ------ | -------- | ----------- |
| slug         | string | No       | URL-friendly slug. |
| public_email | string | No       | Public email address. |
| summary      | string | No       | Biography summary. |
| headline     | string | No       | Professional headline. |

> Parameters accept camelCase, snake_case, kebab-case, or CapitalCase variants.

## Examples

### Request example (curl)

```bash
curl -X PUT \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Content-Type: application/json" \
  -d '{
    "slug": "john-doe-developer",
    "summary": "Experienced software engineer"
  }' \
  "https://sandbox.your-domain.com/api/v1/bio/me"
```

### Response example

```json
{
  "data": {
    "id": 1,
    "uuid": "550e8400-e29b-41d4-a716-446655440000",
    "user_id": 123,
    "slug": "john-doe-developer",
    "public_email": "john.public@example.com",
    "summary": "Experienced software engineer",
    "headline": "Senior Full-Stack Engineer",
    "updated_at": "2025-01-15T12:00:00.000000Z"
  }
}
```

## HTTP Status

- 200: OK
- 401: Unauthorized
- 403: Forbidden
- 422: Unprocessable Entity
- 500: Internal Server Error

## Errors

```json
{
  "message": "The given data was invalid.",
  "errors": {
    "slug": ["The slug has already been taken."]
  }
}
```

## Notes

- All fields are optional in the update request.
- The slug is automatically generated from the provided value.
- Updates only the authenticated user's biography.

## Related

- [My Biography — Show](MeBiographyShow.md)
