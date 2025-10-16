# Bio – My Languages Index

## Endpoint

```
GET /api/v1/bio/me/languages
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
  "https://sandbox.your-domain.com/api/v1/bio/me/languages"
```

### Response example

```json
{
  "data": ["code": "en", "name": "English", "proficiency": "native"]
}
```

## HTTP Status

- 200: OK
- 401: Unauthorized
- 500: Internal Server Error

## Notes

- Returns items for the authenticated user only.

## Related

- [My Languages — Store](MyLanguagesStore.md)
