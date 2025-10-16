# Bio – My Certifications Index

## Endpoint

```
GET /api/v1/bio/me/certifications
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

### Query parameters

| Parameter  | Type    | Required | Description | Default/Values |
| ---------- | ------- | -------- | ----------- | -------------- |
| noPaginate | boolean | No       | Return all records without pagination. | false |
| perPage    | integer | No       | Number of items per page. | 25 |

> Parameters accept camelCase, snake_case, kebab-case, or CapitalCase variants.

## Examples

### Request example (curl)

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: en" \
  "https://sandbox.your-domain.com/api/v1/bio/me/certifications"
```

### Response example

```json
{
  "data": [
    {"uuid": "550e8400-e29b-41d4-a716-446655440000", "title": "AWS Certified", "issued_date": "2024-01-15"}
  ],
  "links": {
    "first": "https://sandbox.your-domain.com/api/v1/bio/me/certifications?page=1",
    "last": "https://sandbox.your-domain.com/api/v1/bio/me/certifications?page=5",
    "prev": null,
    "next": "https://sandbox.your-domain.com/api/v1/bio/me/certifications?page=2"
  },
  "meta": {
    "current_page": 1,
    "per_page": 25,
    "total": 100
  }
}
```

## HTTP Status

- 200: OK
- 401: Unauthorized
- 403: Forbidden
- 429: Too Many Requests
- 500: Internal Server Error

## Errors

```json
{
  "message": "Unauthenticated."
}
```

## Notes

- Returns certifications for the authenticated user only.
- Use `noPaginate=true` to retrieve all records at once.
- Default pagination is 25 items per page.

## Related

- [My Certifications — Show](MyCertificationsShow.md)
- [My Certifications — Store](MyCertificationsStore.md)
- [My Certifications — Update](MyCertificationsUpdate.md)
- [My Certifications — Destroy](MyCertificationsDestroy.md)
