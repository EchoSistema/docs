# Bio – Skills Store

## Endpoint

```
POST /api/v1/bio/skills
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
  "content": "Python",
  "language": "en",
  "is_default": true
}
```

| Field      | Type    | Required | Description |
| ---------- | ------- | -------- | ----------- |
| content    | string  | Yes      | Skill name/title. |
| language   | string  | No       | Language code (e.g., `en`, `pt-BR`). Defaults to platform language. |
| is_default | boolean | No       | Whether this is a default skill. Defaults to false. |

> Parameters accept camelCase, snake_case, kebab-case, or CapitalCase variants.

## Examples

### Request example (curl)

```bash
curl -X POST \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: en" \
  -H "Content-Type: application/json" \
  -d '{
    "content": "Python",
    "language": "en",
    "is_default": true
  }' \
  "https://sandbox.your-domain.com/api/v1/bio/skills"
```

### Response example

```json
{
  "data": {
    "id": 123,
    "uuid": "550e8400-e29b-41d4-a716-446655440000",
    "content": "Python",
    "language": "en",
    "usage": "skill",
    "is_default": true,
    "created_at": "2025-01-15T10:30:00.000000Z",
    "updated_at": "2025-01-15T10:30:00.000000Z"
  }
}
```

## JSON Structure Explained

| Field          | Type    | Description |
| -------------- | ------- | ----------- |
| data           | object  | Created skill object. |
| data.id        | integer | Skill identifier. |
| data.uuid      | string  | Unique skill identifier. |
| data.content   | string  | Skill name/title. |
| data.language  | string  | Language code. |
| data.usage     | string  | Usage type (always "skill"). |
| data.is_default| boolean | Whether this is a default skill. |
| data.created_at| string  | Creation timestamp. |
| data.updated_at| string  | Last update timestamp. |

## HTTP Status

- 201: Created
- 400: Bad Request
- 401: Unauthorized
- 403: Forbidden
- 422: Unprocessable Entity
- 429: Too Many Requests
- 500: Internal Server Error

## Errors

Validation error example:

```json
{
  "message": "The given data was invalid.",
  "errors": {
    "content": ["The content field is required."]
  }
}
```

## Notes

- The `usage` field is automatically set to "skill".
- The authenticated user becomes the creator of the skill.
- Duplicate skill names in the same language may be prevented by validation.

## Related

- [Skills — Index](SkillIndex.md)
- [Skills — Update](SkillUpdate.md)
- [Skills — Destroy](SkillDestroy.md)
