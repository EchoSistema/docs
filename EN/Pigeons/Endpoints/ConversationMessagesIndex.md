# Pigeons – Get Conversation Messages

## Endpoint
```
GET /api/v1/pigeons/chat/conversations/{uuid}/messages
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
| `uuid` | string | Yes | Conversation UUID |

## Examples

### Request
```bash
curl -X GET \
  https://api.example.com/api/v1/pigeons/chat/conversations/550e8400-e29b-41d4-a716-446655440000/messages \
  -H 'Authorization: Bearer {token}' \
  -H 'X-PUBLIC-KEY: {platform-key}' \
  -H 'Accept-Language: en'
```

### Response
```json
{
  "data": [
    {
      "message_id": "msg_123456789",
      "id": "msg_123456789",
      "conversation_uuid": "550e8400-e29b-41d4-a716-446655440000",
      "sender": {
        "uuid": "660e8400-e29b-41d4-a716-446655440001",
        "name": "John Doe",
        "avatar": "https://example.com/avatars/john.jpg"
      },
      "receiver": {
        "uuid": "770e8400-e29b-41d4-a716-446655440002",
        "name": "Current User",
        "avatar": "https://example.com/avatars/user.jpg"
      },
      "context": {
        "text": "Hello, how are you?",
        "type": "text"
      },
      "attachments": [],
      "metadata": {},
      "reactions": [
        {
          "user_uuid": "770e8400-e29b-41d4-a716-446655440002",
          "emoji": "👍",
          "created_at": "2025-11-02T10:35:00+00:00"
        }
      ],
      "status": {
        "sent": true,
        "delivered": true,
        "read": true
      },
      "timestamps": {
        "sent_at": "2025-11-02T10:30:00+00:00",
        "delivered_at": "2025-11-02T10:30:05+00:00",
        "read_at": "2025-11-02T10:31:00+00:00"
      },
      "pbk": "platform_public_key",
      "device": {
        "type": "mobile",
        "os": "Android",
        "os_version": "14",
        "app_version": "1.0.0",
        "model": "Samsung Galaxy S23"
      }
    }
  ],
  "conversation": {
    "uuid": "550e8400-e29b-41d4-a716-446655440000",
    "taggable_type": "App\\Models\\Article",
    "last_message_text": "Hello, how are you?",
    "last_message_at": "2025-11-02T10:30:00+00:00",
    "is_archived": false,
    "created_at": "2025-11-01T08:00:00+00:00",
    "updated_at": "2025-11-02T10:30:00+00:00",
    "taggable_info": {
      "id": 123,
      "title": "Article Title"
    }
  }
}
```

## Explained JSON Structure
| Field | Type | Description |
|-------|------|-------------|
| `data` | array | Array of message objects |
| `data[].message_id` | string | Unique message identifier |
| `data[].conversation_uuid` | string | UUID of the conversation |
| `data[].sender` | object | Sender user information |
| `data[].receiver` | object | Receiver user information |
| `data[].context` | object | Message content and type |
| `data[].context.text` | string | Message text content |
| `data[].context.type` | string | Message type (text, image, file, audio, video) |
| `data[].attachments` | array | Array of attached files |
| `data[].metadata` | object | Additional message metadata |
| `data[].reactions` | array | Array of emoji reactions |
| `data[].status` | object | Message delivery status |
| `data[].timestamps` | object | Message timestamps (sent, delivered, read) |
| `data[].pbk` | string | Platform public key |
| `data[].device` | object | Device information from which message was sent |
| `conversation` | object | Conversation metadata |

## HTTP Status
| Code | Description |
|------|-------------|
| 200 | Success – messages retrieved |
| 401 | Unauthorized – invalid or missing token |
| 403 | Forbidden – user is not a participant in this conversation |
| 404 | Not Found – conversation does not exist |
| 429 | Too Many Requests – rate limit exceeded |
| 500 | Internal Server Error |

## Errors
```json
{
  "message": "Conversation not found"
}
```

## Notes
- Only participants of the conversation can retrieve messages
- Messages are stored in MongoDB for high performance
- The response includes both messages and conversation metadata
- Messages include device information to track which device sent each message
- Reactions are stored as an array and can be added by any participant

## Related
- [List Conversations](./ConversationsIndex.md)
- [Send Message](./MessagesStore.md)
- [Mark Message as Read](./MessagesRead.md)
- [Add Reaction](./MessagesReactionsStore.md)
