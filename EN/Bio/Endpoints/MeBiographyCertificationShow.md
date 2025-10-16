# Bio – My Certifications Show

## Endpoint

```
GET /api/v1/bio/me/certifications/{certification}
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
| certification | string | Yes      | Resource UUID. |

## Examples

### Request example (curl)

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  "https://sandbox.your-domain.com/api/v1/bio/me/certifications/550e8400-e29b-41d4-a716-446655440000"
```

### Response example

```json
{
  "data": {"uuid": "550e8400-e29b-41d4-a716-446655440000", "title": "AWS Certified", "issued_date": "2024-01-15"}
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
  "message": "Resource not found."
}
```

## Notes

- Only returns items owned by the authenticated user.

## Related

- [My Certifications — Index](MyCertificationsIndex.md)
- [My Certifications — Update](MyCertificationsUpdate.md)
- [My Certifications — Destroy](MyCertificationsDestroy.md)
