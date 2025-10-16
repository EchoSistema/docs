# Bio – My Certifications Update

## Endpoint

```
PUT /api/v1/bio/me/certifications/{certification}
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

### Path parameters

| Parameter | Type   | Required | Description |
| --------- | ------ | -------- | ----------- |
| certification | string | Yes      | Resource UUID. |

### Request body

```json
{"title": "AWS Certified Solutions Architect"}
```

> Parameters accept camelCase, snake_case, kebab-case, or CapitalCase variants.

## Examples

### Request example (curl)

```bash
curl -X PUT \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Content-Type: application/json" \
  -d '{"title": "AWS Certified Solutions Architect"}' \
  "https://sandbox.your-domain.com/api/v1/bio/me/certifications/550e8400-e29b-41d4-a716-446655440000"
```

### Response example

```json
{
  "data": {
    "uuid": "550e8400-e29b-41d4-a716-446655440000",
    "updated_at": "2025-01-15T12:00:00.000000Z"
  }
}
```

## HTTP Status

- 200: OK
- 401: Unauthorized
- 403: Forbidden
- 404: Not Found
- 422: Unprocessable Entity
- 500: Internal Server Error

## Errors

```json
{
  "message": "You are not authorized to update this resource."
}
```

## Notes

- Only the owner can update the resource.
- All fields in the request body are optional.

## Related

- [My Certifications — Index](MyCertificationsIndex.md)
- [My Certifications — Show](MyCertificationsShow.md)
- [My Certifications — Destroy](MyCertificationsDestroy.md)
