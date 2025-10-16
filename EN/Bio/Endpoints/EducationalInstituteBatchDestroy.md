# Bio – Educational Institutes Batch Destroy

## Endpoint

```
DELETE /api/v1/bio/educational-institutes
```

## Authentication

Required – Bearer {token} with ability `backoffice` and permission `domain:bio`.

## Headers

| Header           | Type   | Required | Description |
| ---------------- | ------ | -------- | ----------- |
| Authorization    | string | Yes      | `Bearer {token}`. |
| X-PUBLIC-KEY     | string | Yes      | Platform public key. |
| Content-Type     | string | Yes      | `application/json`. |

## Parameters

### Request body

```json
{
  "institutes": [
    "550e8400-e29b-41d4-a716-446655440000",
    "660e8400-e29b-41d4-a716-446655440001"
  ]
}
```

| Field      | Type  | Required | Description |
| ---------- | ----- | -------- | ----------- |
| institutes | array | Yes      | Array of educational institute UUIDs. |

## Examples

### Request example (curl)

```bash
curl -X DELETE \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Content-Type: application/json" \
  -d '{"institutes": ["550e8400-e29b-41d4-a716-446655440000"]}' \
  "https://sandbox.your-domain.com/api/v1/bio/educational-institutes"
```

### Response example

```json
{
  "success": true,
  "message": "2 educational institutes deleted successfully."
}
```

## HTTP Status

- 200: OK
- 401: Unauthorized
- 422: Unprocessable Entity
- 500: Internal Server Error

## Errors

```json
{
  "message": "The given data was invalid.",
  "errors": {
    "institutes": ["The institutes field is required."]
  }
}
```

## Notes

- Only items created by the authenticated user can be deleted.
- Items not found or not owned are silently skipped.

## Related

- [Educational Institutes — Index](EducationalInstituteIndex.md)
- [Educational Institutes — Destroy](EducationalInstituteDestroy.md)
