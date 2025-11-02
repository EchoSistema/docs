# Pigeons – Register Device Token

## Endpoint
```
POST /api/v1/pigeons/chat/devices
```

## Authentication
Required – Bearer token with `backoffice` ability.

## Headers
| Header | Type | Required | Description |
|--------|------|----------|-------------|
| `Authorization` | string | Yes | Bearer {token} |
| `X-PUBLIC-KEY` | string | Yes | Platform public key |
| `Accept-Language` | string | No | Locale for translatable fields (pt-BR, en, es) |
| `Content-Type` | string | Yes | application/json |

## Parameters

### Request Body
| Parameter | Type | Required | Description | Values |
|-----------|------|----------|-------------|--------|
| `token` | string | Yes | Firebase Cloud Messaging (FCM) device token (max 4096 chars) | - |
| `platform` | string | Yes | Device platform | android, ios, web |
| `device.type` | string | No | Device type | - |
| `device.os` | string | No | Operating system name | - |
| `device.os_version` | string | No | Operating system version | - |
| `device.app_version` | string | No | Application version | - |
| `device.model` | string | No | Device model | - |
| `device.browser` | string | No | Browser name (for web) | - |
| `device.browser_version` | string | No | Browser version | - |

## Examples

### Request
```bash
curl -X POST \
  https://api.example.com/api/v1/pigeons/chat/devices \
  -H 'Authorization: Bearer {token}' \
  -H 'X-PUBLIC-KEY: {platform-key}' \
  -H 'Content-Type: application/json' \
  -H 'Accept-Language: en' \
  -d '{
    "token": "fGxR8hJ3T...",
    "platform": "android",
    "device": {
      "type": "mobile",
      "os": "Android",
      "os_version": "14",
      "app_version": "1.0.0",
      "model": "Samsung Galaxy S23"
    }
  }'
```

### Response
```json
{
  "message": "Device token registrado",
  "data": {
    "id": "67890abcdef12345",
    "token": "fGxR8hJ3T...",
    "platform": "android"
  }
}
```

## Explained JSON Structure
| Field | Type | Description |
|-------|------|-------------|
| `message` | string | Success message |
| `data` | object | Registered device token information |
| `data.id` | string | Device token document ID |
| `data.token` | string | FCM device token |
| `data.platform` | string | Device platform |

## HTTP Status
| Code | Description |
|------|-------------|
| 201 | Created – device token registered successfully |
| 400 | Bad Request – invalid parameters |
| 401 | Unauthorized – invalid or missing token |
| 403 | Forbidden – insufficient permissions |
| 422 | Unprocessable Entity – validation errors |
| 429 | Too Many Requests – rate limit exceeded |
| 500 | Internal Server Error |

## Errors
```json
{
  "message": "The given data was invalid.",
  "errors": {
    "token": [
      "The token field is required."
    ],
    "platform": [
      "The platform field is required."
    ]
  }
}
```

## Notes
- Device tokens are used for Firebase Cloud Messaging (FCM) push notifications
- If a token already exists for the same platform and user, it will be deleted and re-created with updated metadata
- Tokens are associated with the authenticated user and the current platform (via `X-PUBLIC-KEY`)
- The maximum token length is 4096 characters (FCM token limit)
- Supported platforms are: `android`, `ios`, and `web`
- Device metadata is optional but recommended for analytics and debugging
- Tokens are stored in MongoDB for efficient querying

## Related
- [Delete Device Token](./DevicesDestroy.md)
- [Send Message](./MessagesStore.md)
- [List Conversations](./ConversationsIndex.md)
