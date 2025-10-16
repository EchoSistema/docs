# Bio – Skills Destroy

## Endpoint

```
DELETE /api/v1/bio/skills/{uuid}
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

### Path parameters

| Parameter | Type   | Required | Description |
| --------- | ------ | -------- | ----------- |
| uuid      | string | Yes      | Skill UUID. |

## Examples

### Request example (curl)

```bash
curl -X DELETE \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: en" \
  "https://sandbox.your-domain.com/api/v1/bio/skills/550e8400-e29b-41d4-a716-446655440000"
```

### Response example

```json
{
  "success": true,
  "message": "Skill 'Python' deleted successfully."
}
```

## JSON Structure Explained

| Field   | Type    | Description |
| ------- | ------- | ----------- |
| success | boolean | Operation success status. |
| message | string  | Success message with skill name. |

## HTTP Status

- 200: OK
- 401: Unauthorized
- 403: Forbidden
- 404: Not Found
- 429: Too Many Requests
- 500: Internal Server Error

## Errors

Not found error example:

```json
{
  "message": "Skill not found."
}
```

Forbidden error example:

```json
{
  "message": "You are not authorized to delete this skill."
}
```

## Notes

- Only the creator of the skill can delete it.
- Deletion is permanent and cannot be undone.
- If the skill is referenced by user biographies, consider the cascade behavior.

## Related

- [Skills — Index](SkillIndex.md)
- [Skills — Store](SkillStore.md)
- [Skills — Batch Destroy](SkillBatchDestroy.md)
