# Auth – Keep-Alive Endpoint

## Endpoint

`GET /api/v1/auth/keep-alive`

Checks whether the provided token is valid and keeps the user session active.

---

## Authentication

Requires a valid Bearer token.

### Required Headers

| Header | Type | Description |
| ------ | ---- | ----------- |
| **Authorization** | `string` | `Bearer <token>` required. |
| **X-PUBLIC-KEY** | `string` | Platform public key required to authenticate and receive responses. |
| **Accept-Language** | `string` | Locale for response messages. |

---

## Example Response

```json
{
  "success": true,
  "message": "User session kept active."
}
```

---

## Notes

* The response message is translated according to `Accept-Language`.

