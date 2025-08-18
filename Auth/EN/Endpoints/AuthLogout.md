# Auth – Logout Endpoint

## Endpoint

`POST /auth/logout`

Ends the user session by invalidating the access token.

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
  "message": "User :name successfully logged out."
}
```

---

## Notes

* The response message is translated according to `Accept-Language`.

