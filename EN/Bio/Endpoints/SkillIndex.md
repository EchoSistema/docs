# Bio – Skills Index

## Endpoint

```
GET /api/v1/bio/skills
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

| Parameter       | Type    | Required | Description | Default/Values |
| --------------- | ------- | -------- | ----------- | -------------- |
| noPaginate      | boolean | No       | Return all records without pagination. | false |
| perPage         | integer | No       | Number of items per page. | 50 |
| skill_language  | string  | No       | Filter skills by language code (e.g., `en`, `pt-BR`). | Uses default language |

> Document the canonical parameter names in `snake_case`. When applicable, mention that variants are accepted (`camelCase`, `kebab-case`, `CapitalCase`).

## Examples

### Request example (curl)

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: en" \
  "https://sandbox.your-domain.com/api/v1/bio/skills?perPage=25"
```

### Response example

```json
{
  "data": [
    {
      "id": 1,
      "uuid": "550e8400-e29b-41d4-a716-446655440000",
      "content": "JavaScript",
      "language": "en",
      "usage": "skill",
      "is_default": true,
      "created_at": "2025-01-15T10:30:00.000000Z",
      "updated_at": "2025-01-15T10:30:00.000000Z"
    }
  ],
  "links": {
    "first": "https://sandbox.your-domain.com/api/v1/bio/skills?page=1",
    "last": "https://sandbox.your-domain.com/api/v1/bio/skills?page=10",
    "prev": null,
    "next": "https://sandbox.your-domain.com/api/v1/bio/skills?page=2"
  },
  "meta": {
    "current_page": 1,
    "from": 1,
    "last_page": 10,
    "per_page": 50,
    "to": 50,
    "total": 500
  }
}
```

## JSON Structure Explained

| Field                | Type    | Description |
| -------------------- | ------- | ----------- |
| data[]               | array   | List of skills. |
| data[].id            | integer | Skill identifier. |
| data[].uuid          | string  | Unique skill identifier. |
| data[].content       | string  | Skill name/title. |
| data[].language      | string  | Language code. |
| data[].usage         | string  | Usage type (always "skill"). |
| data[].is_default    | boolean | Whether this is a default skill. |
| data[].created_at    | string  | Creation timestamp. |
| data[].updated_at    | string  | Last update timestamp. |
| links                | object  | Pagination links (when paginated). |
| meta                 | object  | Pagination metadata (when paginated). |

## HTTP Status

- 200: OK
- 401: Unauthorized
- 403: Forbidden
- 429: Too Many Requests
- 500: Internal Server Error

## Errors

Standard error responses follow this format:

```json
{
  "message": "Unauthenticated."
}
```

## Notes

- By default, the endpoint returns skills filtered by `is_default = true` and default platform language.
- Use `skill_language` parameter to filter by specific language.
- Use `noPaginate=true` to retrieve all records at once (use with caution for large datasets).
- Default pagination is 50 items per page.

## Related

- [Skills — Store](SkillStore.md)
- [Skills — Update](SkillUpdate.md)
- [Skills — Destroy](SkillDestroy.md)
- [Skills — Batch Destroy](SkillBatchDestroy.md)
