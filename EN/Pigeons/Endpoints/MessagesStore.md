# Pigeons – Send Message

## Endpoint
```
POST /api/v1/pigeons/chat/messages
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
| `conversation_uuid` | string | Yes | UUID of the conversation | Valid conversation UUID |
| `receiver_uuid` | string | Yes | UUID of the message recipient | Valid user UUID |
| `context_text` | string | Yes | Message text content (max 5000 chars) | - |
| `context_type` | string | Yes | Type of message | text, image, file, audio, video |
| `attachments` | array | No | Array of file attachments | - |
| `metadata` | object | No | Additional message metadata | - |
| `device.type` | string | Yes | Device type | mobile, web, desktop |
| `device.os` | string | Yes | Operating system name | - |
| `device.os_version` | string | No | Operating system version | - |
| `device.app_version` | string | No | Application version | - |
| `device.model` | string | No | Device model | - |
| `device.browser` | string | No | Browser name (for web/desktop) | - |
| `device.browser_version` | string | No | Browser version | - |

## Examples

### Request
```bash
curl -X POST \
  https://api.example.com/api/v1/pigeons/chat/messages \
  -H 'Authorization: Bearer {token}' \
  -H 'X-PUBLIC-KEY: {platform-key}' \
  -H 'Content-Type: application/json' \
  -H 'Accept-Language: en' \
  -d '{
    "conversation_uuid": "550e8400-e29b-41d4-a716-446655440000",
    "receiver_uuid": "660e8400-e29b-41d4-a716-446655440001",
    "context_text": "Hello, how are you?",
    "context_type": "text",
    "attachments": [],
    "metadata": {},
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
  "message": "Message sent successfully",
  "data": {
    "message_id": "msg_123456789",
    "conversation_uuid": "550e8400-e29b-41d4-a716-446655440000",
    "sender": {
      "uuid": "770e8400-e29b-41d4-a716-446655440002",
      "name": "Current User",
      "avatar": "https://example.com/avatars/user.jpg"
    },
    "receiver": {
      "uuid": "660e8400-e29b-41d4-a716-446655440001",
      "name": "John Doe",
      "avatar": "https://example.com/avatars/john.jpg"
    },
    "context": {
      "text": "Hello, how are you?",
      "type": "text"
    },
    "attachments": [],
    "metadata": {},
    "reactions": [],
    "status": {
      "sent": true,
      "delivered": false,
      "read": false
    },
    "timestamps": {
      "sent_at": "2025-11-02T10:30:00+00:00"
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
}
```

## Explained JSON Structure
| Field | Type | Description |
|-------|------|-------------|
| `message` | string | Success message |
| `data` | object | Created message object |
| `data.message_id` | string | Unique message identifier |
| `data.conversation_uuid` | string | UUID of the conversation |
| `data.sender` | object | Sender user information (authenticated user) |
| `data.receiver` | object | Receiver user information |
| `data.context` | object | Message content and type |
| `data.attachments` | array | Array of attached files |
| `data.metadata` | object | Additional message metadata |
| `data.reactions` | array | Array of emoji reactions (empty for new messages) |
| `data.status` | object | Message delivery status |
| `data.timestamps` | object | Message timestamps |
| `data.pbk` | string | Platform public key |
| `data.device` | object | Device information |

## HTTP Status
| Code | Description |
|------|-------------|
| 201 | Created – message sent successfully |
| 400 | Bad Request – invalid parameters |
| 401 | Unauthorized – invalid or missing token |
| 403 | Forbidden – user is not a participant in this conversation |
| 404 | Not Found – conversation or receiver not found |
| 422 | Unprocessable Entity – validation errors |
| 429 | Too Many Requests – rate limit exceeded |
| 500 | Internal Server Error |

## Errors
```json
{
  "message": "The given data was invalid.",
  "errors": {
    "context_text": [
      "The context text field is required."
    ],
    "device.type": [
      "The device.type field is required."
    ]
  }
}
```

## Notes
- Messages are stored in MongoDB for high performance
- Real-time notifications are sent via WebSockets/broadcasting
- Push notifications are sent to registered device tokens
- The conversation's `last_message_at` and `last_message_text` are automatically updated
- Participant's `unread_count` is automatically incremented for the receiver
- Maximum message length is 5000 characters
- Device information is required to track message sources

## Related
- [List Conversations](./ConversationsIndex.md)
- [Get Conversation Messages](./ConversationMessagesIndex.md)
- [Mark Message as Read](./MessagesRead.md)
- [Add Reaction](./MessagesReactionsStore.md)
