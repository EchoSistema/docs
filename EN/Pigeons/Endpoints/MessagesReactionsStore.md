# Pigeons – Add Reaction to Message

## Endpoint
```
POST /api/v1/pigeons/chat/messages/{messageId}/reactions
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

### Path Parameters
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `messageId` | string | Yes | Message ID |

### Request Body
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `emoji` | string | Yes | Emoji reaction (max 10 characters) |

## Examples

### Request
```bash
curl -X POST \
  https://api.example.com/api/v1/pigeons/chat/messages/msg_123456789/reactions \
  -H 'Authorization: Bearer {token}' \
  -H 'X-PUBLIC-KEY: {platform-key}' \
  -H 'Content-Type: application/json' \
  -H 'Accept-Language: en' \
  -d '{
    "emoji": "👍"
  }'
```

### Response
```json
{
  "message": "Reaction added successfully"
}
```

## Explained JSON Structure
| Field | Type | Description |
|-------|------|-------------|
| `message` | string | Success message |

## HTTP Status
| Code | Description |
|------|-------------|
| 200 | Success – reaction added |
| 400 | Bad Request – invalid parameters |
| 401 | Unauthorized – invalid or missing token |
| 403 | Forbidden – insufficient permissions |
| 404 | Not Found – message does not exist |
| 422 | Unprocessable Entity – validation errors |
| 429 | Too Many Requests – rate limit exceeded |
| 500 | Internal Server Error |

## Errors
```json
{
  "message": "The given data was invalid.",
  "errors": {
    "emoji": [
      "The emoji field is required."
    ]
  }
}
```

```json
{
  "message": "Message not found"
}
```

## Notes
- Any participant in the conversation can add reactions to messages
- The emoji is limited to 10 characters (supports most single emojis and sequences)
- Each user can add multiple different reactions to the same message
- Adding the same emoji again will not create a duplicate
- Reactions are stored in the message's `reactions` array with the user UUID and timestamp
- Real-time updates may be broadcast to notify participants of new reactions

## Related
- [Get Conversation Messages](./ConversationMessagesIndex.md)
- [Send Message](./MessagesStore.md)
- [Mark Message as Read](./MessagesRead.md)
