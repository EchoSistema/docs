# Shared – Subscribe to Newsletter

## Endpoint

```
POST /api/v1/newsletters
```

## Authentication

None

## Headers

| Header     | Type | Required | Description |
| ---------------- | ------ | -------- | ----------- |
| Authorization    | string | No | `Bearer {token}`. |
| X-PUBLIC-KEY     | string | Yes      | Platform public key. |
| Accept-Language  | string | No       | IETF locale (e.g., `pt-BR`, `en`, `es`). |

## Parameters

Subscribe an email to the newsletter

## Examples

### Request example (curl)

```bash
curl -X POST \
  
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: en" \
  "https://sandbox.your-domain.com/api/v1/newsletters"
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

- Subscribe an email to the newsletter

## Related

- See other Shared API endpoints

## Changelog

- 2025-10-16: Initial documentation
