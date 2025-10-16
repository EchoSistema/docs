# Bio – Skills Update

## Endpoint

```
PUT /api/v1/bio/skills/{uuid}
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

### Path parameters

| Parameter | Type   | Required | Description |
| --------- | ------ | -------- | ----------- |
| uuid      | string | Yes      | Skill UUID. |

### Request body

```json
{
  "content": "Python 3.x",
  "language": "en",
  "is_default": true
}
```

| Field      | Type    | Required | Description |
| ---------- | ------- | -------- | ----------- |
| content    | string  | No       | Skill name/title. |
| language   | string  | No       | Language code (e.g., `en`, `pt-BR`). |
| is_default | boolean | No       | Whether this is a default skill. |

> Parameters accept camelCase, snake_case, kebab-case, or CapitalCase variants.

## Examples

### Request example (curl)

```bash
curl -X PUT \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: en" \
  -H "Content-Type: application/json" \
  -d '{
    "content": "Python 3.x",
    "is_default": true
  }' \
  "https://sandbox.your-domain.com/api/v1/bio/skills/550e8400-e29b-41d4-a716-446655440000"
```

### Response example

```json
{
  "data": {
    "id": 123,
    "uuid": "550e8400-e29b-41d4-a716-446655440000",
    "content": "Python 3.x",
    "language": "en",
    "usage": "skill",
    "is_default": true,
    "created_at": "2025-01-15T10:30:00.000000Z",
    "updated_at": "2025-01-15T11:45:00.000000Z"
  }
}
```

## JSON Structure Explained

| Field          | Type    | Description |
| -------------- | ------- | ----------- |
| data           | object  | Updated skill object. |
| data.id        | integer | Skill identifier. |
| data.uuid      | string  | Unique skill identifier. |
| data.content   | string  | Skill name/title. |
| data.language  | string  | Language code. |
| data.usage     | string  | Usage type (always "skill"). |
| data.is_default| boolean | Whether this is a default skill. |
| data.created_at| string  | Creation timestamp. |
| data.updated_at| string  | Last update timestamp. |

## HTTP Status

- 200: OK
- 400: Bad Request
- 401: Unauthorized
- 403: Forbidden
- 404: Not Found
- 422: Unprocessable Entity
- 429: Too Many Requests
- 500: Internal Server Error

## Errors

Not found error example:

```json
{
  "message": "Skill not found."
}
```

Forbidden error example (not creator):

```json
{
  "message": "You are not authorized to update this skill."
}
```

## Notes

- Only the creator of the skill can update it.
- All fields are optional in the update request.
- The `updated_at` timestamp is automatically updated.

## Related

- [Skills — Index](SkillIndex.md)
- [Skills — Store](SkillStore.md)
- [Skills — Destroy](SkillDestroy.md)
