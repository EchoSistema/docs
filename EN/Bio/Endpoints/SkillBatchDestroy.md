# Bio – Skills Batch Destroy

## Endpoint

```
DELETE /api/v1/bio/skills
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
  "skills": [
    "550e8400-e29b-41d4-a716-446655440000",
    "660e8400-e29b-41d4-a716-446655440001"
  ]
}
```

| Field    | Type  | Required | Description |
| -------- | ----- | -------- | ----------- |
| skills   | array | Yes      | Array of skill UUIDs to delete. |

> Parameters accept camelCase, snake_case, kebab-case, or CapitalCase variants.

## Examples

### Request example (curl)

```bash
curl -X DELETE \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: en" \
  -H "Content-Type: application/json" \
  -d '{
    "skills": [
      "550e8400-e29b-41d4-a716-446655440000",
      "660e8400-e29b-41d4-a716-446655440001"
    ]
  }' \
  "https://sandbox.your-domain.com/api/v1/bio/skills"
```

### Response example

```json
{
  "success": true,
  "message": "2 skills deleted successfully."
}
```

## JSON Structure Explained

| Field   | Type    | Description |
| ------- | ------- | ----------- |
| success | boolean | Operation success status. |
| message | string  | Success message with count of deleted skills. |

## HTTP Status

- 200: OK
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
    "skills": ["The skills field is required."]
  }
}
```

## Notes

- Only skills created by the authenticated user can be deleted.
- Skills not found or not owned by the user are silently skipped.
- The response message includes the count of successfully deleted skills.
- Deletion is permanent and cannot be undone.

## Related

- [Skills — Index](SkillIndex.md)
- [Skills — Destroy](SkillDestroy.md)
