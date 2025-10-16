# Shared – List Orders

## Endpoint

```
GET /api/v1/orders
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

List all orders for the current platform

## Examples

### Request example (curl)

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: en" \
  "https://sandbox.your-domain.com/api/v1/orders"
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

- List all orders for the current platform

## Related

- See other Shared API endpoints

## Changelog

- 2025-10-16: Initial documentation
