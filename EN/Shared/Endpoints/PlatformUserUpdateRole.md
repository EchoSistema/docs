# Shared – Update Platform User Role

## Endpoint

```
PATCH /api/v1/platform/users/{user}/roles/{role}
```

## Authentication

Required – Bearer {token}

## Headers

| Header     | Type | Required | Description |
| ---------------- | ------ | -------- | ----------- |
| Authorization    | string | When required | `Bearer {token}`. |
| X-PUBLIC-KEY     | string | Yes      | Platform public key. |
| Accept-Language  | string | No       | IETF locale (e.g., `pt-BR`, `en`, `es`). |

## Parameters

Update or assign a role to a user

## Examples

### Request example (curl)

```bash
curl -X PATCH \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: en" \
  "https://sandbox.your-domain.com/api/v1/platform/users/{user}/roles/{role}"
```

### Response example

```json
{
  "data": {}
}
```

## JSON Structure Explained

| Field | Type | Description |
| ----------- | ------- | ----------- |
| data        | object  | Response data |

## HTTP Status

- 200: OK
- 201: Created
- 400: Bad Request
- 401: Unauthorized
- 403: Forbidden
- 404: Not Found
- 422: Unprocessable Entity
- 429: Too Many Requests
- 500: Internal Server Error

## Errors

```json
{
  "message": "Error message"
}
```

## Notes

- Update or assign a role to a user

## Related

- See other Shared API endpoints

## Changelog

- 2025-10-16: Initial documentation
