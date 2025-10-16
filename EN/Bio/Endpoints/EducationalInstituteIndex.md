# Bio – Educational Institutes Index

## Endpoint

```
GET /api/v1/bio/educational-institutes
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
  "https://sandbox.your-domain.com/api/v1/bio/educational-institutes"
```

### Response example

```json
{
  "data": [
    {
      "id": 1,
      "uuid": "550e8400-e29b-41d4-a716-446655440000",
      "name": "MIT",
      "country": "US",
      "created_at": "2025-01-15T10:30:00.000000Z",
      "updated_at": "2025-01-15T10:30:00.000000Z"
    }
  ],
  "links": {
    "first": "https://sandbox.your-domain.com/api/v1/bio/educational-institutes?page=1",
    "last": "https://sandbox.your-domain.com/api/v1/bio/educational-institutes?page=5",
    "prev": null,
    "next": "https://sandbox.your-domain.com/api/v1/bio/educational-institutes?page=2"
  },
  "meta": {
    "current_page": 1,
    "per_page": 25,
    "total": 125
  }
}
```

## JSON Structure Explained

| Field             | Type    | Description |
| ----------------- | ------- | ----------- |
| data[]            | array   | List of educational institutes. |
| data[].id         | integer | Institute identifier. |
| data[].uuid       | string  | Unique institute identifier. |
| data[].name       | string  | Institute name. |
| data[].country    | string  | Country code. |
| data[].created_at | string  | Creation timestamp. |
| data[].updated_at | string  | Last update timestamp. |

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

- Returns all educational institutes regardless of creator.
- Default pagination is 25 items per page.

## Related

- [Educational Institutes — Show](EducationalInstituteShow.md)
- [Educational Institutes — Store](EducationalInstituteStore.md)
