# Bio – My Books Update

## Endpoint

```
PUT /api/v1/bio/me/books/{uuid}
```

## Authentication

Required – Bearer {token} with ability `backoffice` and permission `domain:bio`.

## Headers

| Header           | Type   | Required | Description |
| ---------------- | ------ | -------- | ----------- |
| Authorization    | string | Yes      | `Bearer {token}`. |
| X-PUBLIC-KEY     | string | Yes      | Platform public key. |
| Content-Type     | string | Yes      | `application/json`. |

## HTTP Status

- 200: OK
- 401: Unauthorized
- 404: Not Found
- 422: Unprocessable Entity
- 500: Internal Server Error

## Related

- [My Books — Index](MyBooksIndex.md)
