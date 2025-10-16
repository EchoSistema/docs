# Bio – My Publications Batch Destroy

## Endpoint

```
DELETE /api/v1/bio/me/publications
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
  "publications": ["uuid1", "uuid2"]
}
```

## HTTP Status

- 200: OK
- 401: Unauthorized
- 422: Unprocessable Entity
- 500: Internal Server Error

## Related

- [My Publications — Index](MyPublicationsIndex.md)
