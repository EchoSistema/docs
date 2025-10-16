# Bio – Educational Institutes Update

## Endpoint

```
PUT /api/v1/bio/educational-institutes/{uuid}
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
| uuid      | string | Yes      | Educational institute UUID. |

### Request body

```json
{
  "name": "Massachusetts Institute of Technology"
}
```

| Field   | Type   | Required | Description |
| ------- | ------ | -------- | ----------- |
| name    | string | No       | Institute name. |
| country | string | No       | Country code (ISO 2-letter). |

## Examples

### Request example (curl)

```bash
curl -X PUT \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Content-Type: application/json" \
  -d '{"name": "Massachusetts Institute of Technology"}' \
  "https://sandbox.your-domain.com/api/v1/bio/educational-institutes/550e8400-e29b-41d4-a716-446655440000"
```

### Response example

```json
{
  "data": {
    "id": 1,
    "uuid": "550e8400-e29b-41d4-a716-446655440000",
    "name": "Massachusetts Institute of Technology",
    "country": "US",
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
  "message": "You are not authorized to update this educational institute."
}
```

## Notes

- Only the creator can update the educational institute.

## Related

- [Educational Institutes — Index](EducationalInstituteIndex.md)
- [Educational Institutes — Destroy](EducationalInstituteDestroy.md)
