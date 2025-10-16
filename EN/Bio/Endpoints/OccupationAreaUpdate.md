# Bio – Occupation Areas Update

## Endpoint

```
PUT /api/v1/bio/occupation-areas/{uuid}
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

### Path parameters

| Parameter | Type   | Required | Description |
| --------- | ------ | -------- | ----------- |
| uuid      | string | Yes      | Occupation area UUID. |

### Request body

```json
{
  "titles": [
    {
      "content": "Software Engineering",
      "language": "en"
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
curl -X PUT \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Content-Type: application/json" \
  -d '{
    "titles": [
      {"content": "Software Engineering", "language": "en"}
    ]
  }' \
  "https://sandbox.your-domain.com/api/v1/bio/occupation-areas/550e8400-e29b-41d4-a716-446655440000"
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
        "content": "Software Engineering",
        "language": "en"
      }
    ],
    "created_at": "2025-01-15T10:30:00.000000Z",
    "updated_at": "2025-01-15T12:00:00.000000Z"
  }
}
```

## JSON Structure Explained

| Field                  | Type    | Description |
| ---------------------- | ------- | ----------- |
| data                   | object  | Updated occupation area. |
| data.id                | integer | Occupation area identifier. |
| data.uuid              | string  | Unique occupation area identifier. |
| data.titles[]          | array   | Multi-language titles. |
| data.created_at        | string  | Creation timestamp. |
| data.updated_at        | string  | Last update timestamp. |

## HTTP Status

- 200: OK
- 401: Unauthorized
- 403: Forbidden
- 404: Not Found
- 422: Unprocessable Entity
- 429: Too Many Requests
- 500: Internal Server Error

## Errors

Forbidden error example:

```json
{
  "message": "You are not authorized to update this occupation area."
}
```

## Notes

- Only the creator can update the occupation area.
- Existing titles can be updated and new translations can be added.

## Related

- [Occupation Areas — Index](OccupationAreaIndex.md)
- [Occupation Areas — Store](OccupationAreaStore.md)
- [Occupation Areas — Destroy](OccupationAreaDestroy.md)
