# Bio – Occupation Areas Destroy

## Endpoint

```
DELETE /api/v1/bio/occupation-areas/{uuid}
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
| uuid      | string | Yes      | Occupation area UUID. |

## Examples

### Request example (curl)

```bash
curl -X DELETE \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  "https://sandbox.your-domain.com/api/v1/bio/occupation-areas/550e8400-e29b-41d4-a716-446655440000"
```

### Response example

```json
{
  "success": true,
  "message": "Occupation area 'Software Development' deleted successfully."
}
```

## JSON Structure Explained

| Field   | Type    | Description |
| ------- | ------- | ----------- |
| success | boolean | Operation success status. |
| message | string  | Success message. |

## HTTP Status

- 200: OK
- 401: Unauthorized
- 403: Forbidden
- 404: Not Found
- 429: Too Many Requests
- 500: Internal Server Error

## Errors

Forbidden error example:

```json
{
  "message": "You are not authorized to delete this occupation area."
}
```

## Notes

- Only the creator can delete the occupation area.
- Deletion is permanent and cannot be undone.

## Related

- [Occupation Areas — Index](OccupationAreaIndex.md)
- [Occupation Areas — Batch Destroy](OccupationAreaBatchDestroy.md)
