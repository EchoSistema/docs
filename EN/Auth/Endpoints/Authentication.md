<!-- markdownlint-disable MD013 -->

# Authentication – Authenticate User

## Endpoint

`POST /api/v1/auth`

Authenticates a user.

---

## Authentication

Basic Auth. Send the header `Authorization: Basic <credentials>`, where `<credentials>` is the result of `base64(username:password)`.

---

## Request

### Required Headers

| Header | Type | Description |
| ------- | ---- | ----------- |
| **X-PUBLIC-KEY** | `string` | Platform public key. |
| **Accept-Language** | `string` | Application language (e.g., `en`, `pt-BR`). Optional. |
| **Authorization** | `string` | Basic Auth credentials. |

### Body

| Field | Type | Required | Description |
| ----- | ---- | -------- | ----------- |
| `device` | `string` | Yes | Accessing device identifier. |

---

## Request Example

```http
POST /api/v1/auth HTTP/1.1
Authorization: Basic am9hbzpzZW5oYQ==
X-PUBLIC-KEY: 00000000-0000-0000-0000-000000000000
Accept-Language: en
Content-Type: application/json

{
  "device": "Mozilla/5.0 (Windows NT 10.0)"
}
```

---

## Response Example

```json
{
  "message": "User authenticated successfully!",
  "token": "0001|tokenSample",
  "data": {
    "user": {
      "uuid": "00000000-0000-0000-0000-000000000000",
      "echo_uuid": "e1c1h1o1111111111111111111111111111",
      "name": "Sample User",
      "email": "sample.user@domain.com",
      "avatar": {
        "url": "https://example.com/avatar.png",
        "usage": "avatar"
      },
      "language": "en",
      "roles": [
        {
          "id": 1,
          "platform": {
            "uuid": "22222222-2222-2222-2222-222222222222",
            "name": "Sample Platform",
            "public_key": "33333333-3333-3333-3333-333333333333"
          },
          "name": "guest",
          "localized_name": "Guest",
          "permissions": [
            {
              "subject": "complaint",
              "action": "store"
            }
          ]
        }
      ]
    }
  }
}
```

---

## JSON Structure

| Field | Type | Description |
| ----- | ---- | ----------- |
| `message` | `string` | Success message. |
| `token` | `string` | Access token. |
| `data.user.uuid` | `uuid` | User identifier. |
| `data.user.echo_uuid` | `uuid` | Internal identifier. |
| `data.user.name` | `string` | User name. |
| `data.user.email` | `string` | User email. |
| `data.user.avatar.url` | `string` | Avatar URL. |
| `data.user.avatar.usage` | `string` | Image usage. |
| `data.user.language` | `string` | Preferred language. |
| `data.user.roles[]` | `array` | User roles. |
| `data.user.roles[].name` | `string` | Role name. |
| `data.user.roles[].localized_name` | `string` | Localized role name. |
| `data.user.roles[].permissions[]` | `array` | Role permissions. |
| `data.user.roles[].permissions[].subject` | `string` | Permission domain. |
| `data.user.roles[].permissions[].action` | `string` | Allowed action. |

---

## Notes

* The token must be sent in future authenticated requests.
* The `device` field helps identify the accessing device.

<!-- markdownlint-enable MD013 -->
