# Bio – Occupation Areas Index

## Endpoint

```
GET /api/v1/bio/occupation-areas
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
| perPage    | integer | No       | Number of items per page. | 50 |

> Parameters accept camelCase, snake_case, kebab-case, or CapitalCase variants.

## Examples

### Request example (curl)

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: en" \
  "https://sandbox.your-domain.com/api/v1/bio/occupation-areas"
```

### Response example

```json
{
  "data": [
    {
      "id": 1,
      "uuid": "550e8400-e29b-41d4-a716-446655440000",
      "titles": [
        {
          "id": 10,
          "content": "Software Development",
          "language": "en"
        },
        {
          "id": 11,
          "content": "Desenvolvimento de Software",
          "language": "pt-BR"
        }
      ],
      "created_at": "2025-01-15T10:30:00.000000Z",
      "updated_at": "2025-01-15T10:30:00.000000Z"
    }
  ],
  "links": {
    "first": "https://sandbox.your-domain.com/api/v1/bio/occupation-areas?page=1",
    "last": "https://sandbox.your-domain.com/api/v1/bio/occupation-areas?page=5",
    "prev": null,
    "next": "https://sandbox.your-domain.com/api/v1/bio/occupation-areas?page=2"
  },
  "meta": {
    "current_page": 1,
    "from": 1,
    "last_page": 5,
    "per_page": 50,
    "to": 50,
    "total": 250
  }
}
```

## JSON Structure Explained

| Field                  | Type    | Description |
| ---------------------- | ------- | ----------- |
| data[]                 | array   | List of occupation areas. |
| data[].id              | integer | Occupation area identifier. |
| data[].uuid            | string  | Unique occupation area identifier. |
| data[].titles[]        | array   | Multi-language titles. |
| data[].titles[].id     | integer | Title identifier. |
| data[].titles[].content| string  | Title in specific language. |
| data[].titles[].language| string | Language code. |
| data[].created_at      | string  | Creation timestamp. |
| data[].updated_at      | string  | Last update timestamp. |
| links                  | object  | Pagination links (when paginated). |
| meta                   | object  | Pagination metadata (when paginated). |

## HTTP Status

- 200: OK
- 401: Unauthorized
- 403: Forbidden
- 429: Too Many Requests
- 500: Internal Server Error

## Errors

Standard error responses:

```json
{
  "message": "Unauthenticated."
}
```

## Notes

- Occupation areas are returned with all available language translations.
- Use `noPaginate=true` to retrieve all records at once.
- Default pagination is 50 items per page.

## Related

- [Occupation Areas — Store](OccupationAreaStore.md)
- [Occupation Areas — Update](OccupationAreaUpdate.md)
- [Occupation Areas — Destroy](OccupationAreaDestroy.md)
