# Pigeons – Mark Message as Read

## Endpoint
```
PATCH /api/v1/pigeons/chat/messages/{messageId}/read
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
| `messageId` | string | Yes | Message ID |

## Examples

### Request
```bash
curl -X PATCH \
  https://api.example.com/api/v1/pigeons/chat/messages/msg_123456789/read \
  -H 'Authorization: Bearer {token}' \
  -H 'X-PUBLIC-KEY: {platform-key}' \
  -H 'Accept-Language: en'
```

### Response
```json
{
  "message": "Message marked as read"
}
```

## Explained JSON Structure
| Field | Type | Description |
|-------|------|-------------|
| `message` | string | Success message |

## HTTP Status
| Code | Description |
|------|-------------|
| 200 | Success – message marked as read |
| 401 | Unauthorized – invalid or missing token |
| 403 | Forbidden – user is not the receiver of this message |
| 404 | Not Found – message does not exist |
| 429 | Too Many Requests – rate limit exceeded |
| 500 | Internal Server Error |

## Errors
```json
{
  "message": "Message not found"
}
```

```json
{
  "message": "Unauthorized"
}
```

## Notes
- Only the message receiver can mark a message as read
- The message `status.read` is set to `true`
- The `timestamps.read_at` is set to the current timestamp
- A real-time read receipt event is broadcast to the conversation channel
- The receiver's `unread_count` for this conversation is decremented
- If the message was already marked as read, the operation is idempotent

## Related
- [Get Conversation Messages](./ConversationMessagesIndex.md)
- [Send Message](./MessagesStore.md)
- [List Conversations](./ConversationsIndex.md)
