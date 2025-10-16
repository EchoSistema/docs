# Bio – Educational Institutes Destroy

## Endpoint

```
DELETE /api/v1/bio/educational-institutes/{uuid}
```

## Authentication

Required – Bearer {token} with ability `backoffice` and permission `domain:bio`.

## Headers

| Header           | Type   | Required | Description |
| ---------------- | ------ | -------- | ----------- |
| Authorization    | string | Yes      | `Bearer {token}`. |
| X-PUBLIC-KEY     | string | Yes      | Platform public key. |

## Parameters

### Path parameters

| Parameter | Type   | Required | Description |
| --------- | ------ | -------- | ----------- |
| uuid      | string | Yes      | Educational institute UUID. |

## Examples

### Request example (curl)

```bash
curl -X DELETE \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  "https://sandbox.your-domain.com/api/v1/bio/educational-institutes/550e8400-e29b-41d4-a716-446655440000"
```

### Response example

```json
{
  "success": true,
  "message": "Educational institute 'MIT' deleted successfully."
}
```

## HTTP Status

- 200: OK
- 401: Unauthorized
- 403: Forbidden
- 404: Not Found
- 500: Internal Server Error

## Errors

```json
{
  "message": "You are not authorized to delete this educational institute."
}
```

## Notes

- Only the creator can delete the educational institute.
- Deletion is permanent.

## Related

- [Educational Institutes — Index](EducationalInstituteIndex.md)
- [Educational Institutes — Batch Destroy](EducationalInstituteBatchDestroy.md)
