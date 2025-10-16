# Bio – My Languages Destroy

## Endpoint

```
DELETE /api/v1/bio/me/languages/{language}
```

## Authentication

Required – Bearer {token} with ability `backoffice` and permission `domain:bio`.

## Headers

| Header           | Type   | Required | Description |
| ---------------- | ------ | -------- | ----------- |
| Authorization    | string | Yes      | `Bearer {token}`. |
| X-PUBLIC-KEY     | string | Yes      | Platform public key. |

## HTTP Status

- 200: OK
- 401: Unauthorized
- 404: Not Found
- 500: Internal Server Error

## Related

- [My Languages — Index](MyLanguagesIndex.md)
