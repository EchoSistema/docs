# Bio – My Certifications Destroy

## Endpoint

```
DELETE /api/v1/bio/me/certifications/{certification}
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
| certification | string | Yes      | Resource UUID. |

## Examples

### Request example (curl)

```bash
curl -X DELETE \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  "https://sandbox.your-domain.com/api/v1/bio/me/certifications/550e8400-e29b-41d4-a716-446655440000"
```

### Response example

```json
{
  "success": true,
  "message": "Certification deleted successfully."
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
  "message": "You are not authorized to delete this resource."
}
```

## Notes

- Only the owner can delete the resource.
- Deletion is permanent and cannot be undone.

## Related

- [My Certifications — Index](MyCertificationsIndex.md)
- [My Certifications — Batch Destroy](MyCertificationsBatchDestroy.md)
