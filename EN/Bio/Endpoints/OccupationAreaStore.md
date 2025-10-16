# Bio – Occupation Areas Store

## Endpoint

```
POST /api/v1/bio/occupation-areas
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
{
  "titles": [
    {
      "content": "Software Development",
      "language": "en"
    },
    {
      "content": "Desenvolvimento de Software",
      "language": "pt-BR"
    }
  ]
}
```

| Field             | Type   | Required | Description |
| ----------------- | ------ | -------- | ----------- |
| titles            | array  | Yes      | Array of titles in different languages. |
| titles[].content  | string | Yes      | Title text. |
| titles[].language | string | Yes      | Language code (e.g., `en`, `pt-BR`). |

> Parameters accept camelCase, snake_case, kebab-case, or CapitalCase variants.

## Examples

### Request example (curl)

```bash
curl -X POST \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: en" \
  -H "Content-Type: application/json" \
  -d '{
    "titles": [
      {"content": "Software Development", "language": "en"},
      {"content": "Desenvolvimento de Software", "language": "pt-BR"}
    ]
  }' \
  "https://sandbox.your-domain.com/api/v1/bio/occupation-areas"
```

### Response example

```json
{
  "data": {
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
}
```

## JSON Structure Explained

| Field                  | Type    | Description |
| ---------------------- | ------- | ----------- |
| data                   | object  | Created occupation area. |
| data.id                | integer | Occupation area identifier. |
| data.uuid              | string  | Unique occupation area identifier. |
| data.titles[]          | array   | Multi-language titles. |
| data.titles[].id       | integer | Title identifier. |
| data.titles[].content  | string  | Title in specific language. |
| data.titles[].language | string  | Language code. |
| data.created_at        | string  | Creation timestamp. |
| data.updated_at        | string  | Last update timestamp. |

## HTTP Status

- 201: Created
- 400: Bad Request
- 401: Unauthorized
- 403: Forbidden
- 422: Unprocessable Entity
- 429: Too Many Requests
- 500: Internal Server Error

## Errors

Validation error example:

```json
{
  "message": "The given data was invalid.",
  "errors": {
    "titles": ["The titles field is required."]
  }
}
```

## Notes

- At least one title is required.
- Multiple language translations can be provided in a single request.
- The authenticated user becomes the creator.

## Related

- [Occupation Areas — Index](OccupationAreaIndex.md)
- [Occupation Areas — Update](OccupationAreaUpdate.md)
