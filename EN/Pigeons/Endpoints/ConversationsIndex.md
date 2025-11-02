# Pigeons – List Conversations

## Endpoint
```
GET /api/v1/pigeons/chat/conversations
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
No parameters required.

## Examples

### Request
```bash
curl -X GET \
  https://api.example.com/api/v1/pigeons/chat/conversations \
  -H 'Authorization: Bearer {token}' \
  -H 'X-PUBLIC-KEY: {platform-key}' \
  -H 'Accept-Language: en'
```

### Response
```json
{
  "data": [
    {
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
      },
      "participants": [
        {
          "uuid": "660e8400-e29b-41d4-a716-446655440001",
          "name": "John Doe",
          "email": "john@example.com",
          "avatar": "https://example.com/avatars/john.jpg",
          "avatar_url": "https://example.com/avatars/john.jpg",
          "pivot": {
            "unread_count": 3,
            "last_read_at": "2025-11-02T09:00:00+00:00",
            "joined_at": "2025-11-01T08:00:00+00:00"
          }
        }
      ]
    }
  ]
}
```

## Explained JSON Structure
| Field | Type | Description |
|-------|------|-------------|
| `data` | array | Array of conversation objects |
| `data[].uuid` | string | Unique conversation identifier |
| `data[].taggable_type` | string | Related entity type (polymorphic) |
| `data[].last_message_text` | string | Text of the last message |
| `data[].last_message_at` | string | ISO 8601 timestamp of last message |
| `data[].is_archived` | boolean | Conversation archive status |
| `data[].created_at` | string | ISO 8601 creation timestamp |
| `data[].updated_at` | string | ISO 8601 update timestamp |
| `data[].taggable_info` | object | Information about related entity |
| `data[].participants` | array | Array of participant users |
| `data[].participants[].uuid` | string | User unique identifier |
| `data[].participants[].name` | string | User full name |
| `data[].participants[].email` | string | User email address |
| `data[].participants[].avatar` | string | User avatar URL |
| `data[].participants[].pivot.unread_count` | integer | Number of unread messages for this participant |
| `data[].participants[].pivot.last_read_at` | string | ISO 8601 timestamp of last read |
| `data[].participants[].pivot.joined_at` | string | ISO 8601 timestamp when user joined conversation |

## HTTP Status
| Code | Description |
|------|-------------|
| 200 | Success – conversations retrieved |
| 401 | Unauthorized – invalid or missing token |
| 403 | Forbidden – insufficient permissions |
| 429 | Too Many Requests – rate limit exceeded |
| 500 | Internal Server Error |

## Notes
- Conversations are ordered by `last_message_at` descending (most recent first)
- Only returns conversations where the authenticated user is a participant
- Includes participants with their avatar images and unread message counts
- The `taggable_type` and `taggable_id` allow conversations to be associated with other entities (articles, properties, etc.)

## Related
- [Create Conversation](./ConversationsStore.md)
- [Get Conversation Messages](./ConversationMessagesIndex.md)
- [Send Message](./MessagesStore.md)
