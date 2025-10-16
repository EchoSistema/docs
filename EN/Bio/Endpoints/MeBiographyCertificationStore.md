# Bio – My Certifications Store

## Endpoint

```
POST /api/v1/bio/me/certifications
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
{"title": "AWS Certified", "issued_date": "2024-01-15", "issuer": "Amazon"}
```

> Parameters accept camelCase, snake_case, kebab-case, or CapitalCase variants.

## Examples

### Request example (curl)

```bash
curl -X POST \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Content-Type: application/json" \
  -d '{"title": "AWS Certified", "issued_date": "2024-01-15", "issuer": "Amazon"}' \
  "https://sandbox.your-domain.com/api/v1/bio/me/certifications"
```

### Response example

```json
{
  "data": {"uuid": "550e8400-e29b-41d4-a716-446655440000", "title": "AWS Certified"}
}
```

## HTTP Status

- 201: Created
- 400: Bad Request
- 401: Unauthorized
- 422: Unprocessable Entity
- 500: Internal Server Error

## Errors

```json
{
  "message": "The given data was invalid.",
  "errors": {
    "field": ["Validation error message."]
  }
}
```

## Notes

- Creates a new resource for the authenticated user.

## Related

- [My Certifications — Index](MyCertificationsIndex.md)
- [My Certifications — Show](MyCertificationsShow.md)
- [My Certifications — Update](MyCertificationsUpdate.md)
