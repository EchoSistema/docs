# Bio – Occupation Areas Batch Destroy

## Endpoint

```
DELETE /api/v1/bio/occupation-areas
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
  "areas": [
    "550e8400-e29b-41d4-a716-446655440000",
    "660e8400-e29b-41d4-a716-446655440001"
  ]
}
```

| Field  | Type  | Required | Description |
| ------ | ----- | -------- | ----------- |
| areas  | array | Yes      | Array of occupation area UUIDs to delete. |

> Parameters accept camelCase, snake_case, kebab-case, or CapitalCase variants.

## Examples

### Request example (curl)

```bash
curl -X DELETE \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Content-Type: application/json" \
  -d '{
    "areas": [
      "550e8400-e29b-41d4-a716-446655440000",
      "660e8400-e29b-41d4-a716-446655440001"
    ]
  }' \
  "https://sandbox.your-domain.com/api/v1/bio/occupation-areas"
```

### Response example

```json
{
  "success": true,
  "message": "2 occupation areas deleted successfully."
}
```

## JSON Structure Explained

| Field   | Type    | Description |
| ------- | ------- | ----------- |
| success | boolean | Operation success status. |
| message | string  | Success message with count. |

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
    "areas": ["The areas field is required."]
  }
}
```

## Notes

- Only occupation areas created by the authenticated user can be deleted.
- Items not found or not owned by the user are silently skipped.
- The response includes the count of successfully deleted items.

## Related

- [Occupation Areas — Index](OccupationAreaIndex.md)
- [Occupation Areas — Destroy](OccupationAreaDestroy.md)
