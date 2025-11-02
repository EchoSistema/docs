# Pigeons – Create Conversation

## Endpoint
```
POST /api/v1/pigeons/chat/conversations
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
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `participant_uuid` | string | Yes | UUID of the user to start conversation with |
| `taggable_type` | string | No | Type of related entity (e.g., "App\\Models\\Article") |
| `taggable_id` | integer | No | ID of related entity |

## Examples

### Request
```bash
curl -X POST \
  https://api.example.com/api/v1/pigeons/chat/conversations \
  -H 'Authorization: Bearer {token}' \
  -H 'X-PUBLIC-KEY: {platform-key}' \
  -H 'Content-Type: application/json' \
  -H 'Accept-Language: en' \
  -d '{
    "participant_uuid": "660e8400-e29b-41d4-a716-446655440001",
    "taggable_type": "App\\Models\\Article",
    "taggable_id": 123
  }'
```

### Response
```json
{
  "message": "Conversation created successfully",
  "data": {
    "uuid": "550e8400-e29b-41d4-a716-446655440000",
    "taggable_type": "App\\Models\\Article",
    "taggable_id": 123,
    "last_message_text": null,
    "last_message_at": null,
    "is_archived": false,
    "created_at": "2025-11-02T10:30:00+00:00",
    "updated_at": "2025-11-02T10:30:00+00:00",
    "participants": [
      {
        "uuid": "770e8400-e29b-41d4-a716-446655440002",
        "name": "Current User",
        "email": "user@example.com"
      },
      {
        "uuid": "660e8400-e29b-41d4-a716-446655440001",
        "name": "John Doe",
        "email": "john@example.com"
      }
    ]
  }
}
```

## Explained JSON Structure
| Field | Type | Description |
|-------|------|-------------|
| `message` | string | Success message |
| `data` | object | Created conversation object |
| `data.uuid` | string | Unique conversation identifier |
| `data.taggable_type` | string | Related entity type |
| `data.taggable_id` | integer | Related entity ID |
| `data.last_message_text` | string\|null | Text of the last message (null for new conversations) |
| `data.last_message_at` | string\|null | ISO 8601 timestamp of last message |
| `data.is_archived` | boolean | Conversation archive status |
| `data.created_at` | string | ISO 8601 creation timestamp |
| `data.updated_at` | string | ISO 8601 update timestamp |
| `data.participants` | array | Array of participant users |

## HTTP Status
| Code | Description |
|------|-------------|
| 201 | Created – conversation successfully created |
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
    "participant_uuid": [
      "The participant uuid field is required."
    ]
  }
}
```

## Notes
- If a conversation already exists between the two users for the same `taggable_type` and `taggable_id`, the existing conversation may be returned instead of creating a duplicate
- The authenticated user is automatically added as a participant
- The `taggable_type` and `taggable_id` are optional and allow associating the conversation with a specific entity
- The `participant_uuid` must be a valid existing user UUID

## Related
- [List Conversations](./ConversationsIndex.md)
- [Get Conversation Messages](./ConversationMessagesIndex.md)
- [Send Message](./MessagesStore.md)
