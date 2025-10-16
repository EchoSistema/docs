# Bio – Educational Institutes Store

## Endpoint

```
POST /api/v1/bio/educational-institutes
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

### Request body

```json
{
  "name": "MIT",
  "country": "US"
}
```

| Field   | Type   | Required | Description |
| ------- | ------ | -------- | ----------- |
| name    | string | Yes      | Institute name. |
| country | string | Yes      | Country code (ISO 2-letter). |

> Parameters accept camelCase, snake_case, kebab-case, or CapitalCase variants.

## Examples

### Request example (curl)

```bash
curl -X POST \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Content-Type: application/json" \
  -d '{"name": "MIT", "country": "US"}' \
  "https://sandbox.your-domain.com/api/v1/bio/educational-institutes"
```

### Response example

```json
{
  "data": {
    "id": 1,
    "uuid": "550e8400-e29b-41d4-a716-446655440000",
    "name": "MIT",
    "country": "US",
    "created_at": "2025-01-15T10:30:00.000000Z",
    "updated_at": "2025-01-15T10:30:00.000000Z"
  }
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
    "name": ["The name field is required."]
  }
}
```

## Notes

- The authenticated user becomes the creator.

## Related

- [Educational Institutes — Index](EducationalInstituteIndex.md)
- [Educational Institutes — Update](EducationalInstituteUpdate.md)
