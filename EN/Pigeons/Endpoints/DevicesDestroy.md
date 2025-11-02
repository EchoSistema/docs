# Pigeons – Delete Device Token

## Endpoint
```
DELETE /api/v1/pigeons/chat/devices/{token}
```

## Authentication
Required – Bearer token with `backoffice` ability.

## Headers
| Header | Type | Required | Description |
|--------|------|----------|-------------|
| `Authorization` | string | Yes | Bearer {token} |
| `X-PUBLIC-KEY` | string | Yes | Platform public key |
| `Accept-Language` | string | No | Locale for translatable fields (pt-BR, en, es) |

## Parameters

### Path Parameters
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `token` | string | Yes | FCM device token to delete |

## Examples

### Request
```bash
curl -X DELETE \
  https://api.example.com/api/v1/pigeons/chat/devices/fGxR8hJ3T... \
  -H 'Authorization: Bearer {token}' \
  -H 'X-PUBLIC-KEY: {platform-key}' \
  -H 'Accept-Language: en'
```

### Response
```json
{
  "message": "Device token removido"
}
```

## Explained JSON Structure
| Field | Type | Description |
|-------|------|-------------|
| `message` | string | Success message |

## HTTP Status
| Code | Description |
|------|-------------|
| 200 | Success – device token deleted |
| 401 | Unauthorized – invalid or missing token |
| 403 | Forbidden – insufficient permissions |
| 429 | Too Many Requests – rate limit exceeded |
| 500 | Internal Server Error |

## Notes
- Deletes the device token for the authenticated user and current platform
- Only the token owner can delete their own tokens
- If the token doesn't exist, the operation completes successfully (idempotent)
- After deletion, push notifications will no longer be sent to this device
- Users should call this endpoint when:
  - Logging out of the application
  - Uninstalling the application
  - Disabling push notifications
- The token is matched against the user UUID and platform public key for security

## Related
- [Register Device Token](./DevicesStore.md)
- [Send Message](./MessagesStore.md)
- [List Conversations](./ConversationsIndex.md)
