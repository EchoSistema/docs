# Chat – List Conversations

## Endpoint

`GET /api/v1/chat/conversations`

Retrieves a list of conversations for the authenticated user, ordered by last message date.

---

## Authentication

**Required** – Bearer Token.

---

## Request

### Required Headers

| Header            | Type     | Description |
| ----------------- | -------- | ----------- |
| **Authorization** | `string` | `Bearer {token}` credential. **Required**. |
| **X-PUBLIC-KEY**  | `string` | Public key identifying the requesting platform. **Required**. |

---

## Response Example

```json
{
  "data": [
    {
      "uuid": "9b732d6a-2e56-4db8-9b2e-6fd8c14e7f28",
      "taggable_type": "App\\Models\\Example",
      "last_message_text": "Hello, how are you?",
      "last_message_at": "2025-10-15T10:30:00.000000Z",
      "is_archived": false,
      "created_at": "2025-10-01T08:00:00.000000Z",
      "updated_at": "2025-10-15T10:30:00.000000Z",
      "taggable_info": {
        "type": "Example",
        "uuid": "8a621c5b-1d45-4cb7-8a1d-5ec7b13d6e17",
        "title": "Example Context"
      },
      "participants": [
        {
          "uuid": "7c531b4a-1c34-4ba6-7b0c-4db6a12c5d16",
          "name": "John Smith",
          "email": "john@example.com",
          "pivot": {
            "conversation_id": 1,
            "user_id": 10,
            "unread_count": 2,
            "last_read_at": "2025-10-15T09:00:00.000000Z",
            "joined_at": "2025-10-01T08:00:00.000000Z",
            "created_at": "2025-10-01T08:00:00.000000Z",
            "updated_at": "2025-10-15T09:00:00.000000Z"
          }
        },
        {
          "uuid": "6b420a39-0b23-3a95-6a0b-3ca5900b4c05",
          "name": "Mary Johnson",
          "email": "mary@example.com",
          "pivot": {
            "conversation_id": 1,
            "user_id": 11,
            "unread_count": 0,
            "last_read_at": "2025-10-15T10:30:00.000000Z",
            "joined_at": "2025-10-01T08:00:00.000000Z",
            "created_at": "2025-10-01T08:00:00.000000Z",
            "updated_at": "2025-10-15T10:30:00.000000Z"
          }
        }
      ]
    }
  ]
}
```

---

## JSON Structure Explained

### `data[]` – Conversations Array

| Field               | Type      | Description |
| ------------------- | --------- | ----------- |
| `uuid`              | `uuid`    | Unique identifier for the conversation. |
| `taggable_type`     | `string`  | Type of polymorphic model associated with the conversation (when applicable). |
| `last_message_text` | `string`  | Text of the last message sent in the conversation. |
| `last_message_at`   | `string`  | Date and time of the last message (ISO-8601 format). |
| `is_archived`       | `boolean` | Indicates whether the conversation is archived. |
| `created_at`        | `string`  | Conversation creation date (ISO-8601 format). |
| `updated_at`        | `string`  | Last update date of the conversation (ISO-8601 format). |
| `taggable_info`     | `object`  | Information about the context associated with the conversation. |
| `participants[]`    | `array`   | List of conversation participants. |

### `taggable_info`

| Field   | Type     | Description |
| ------- | -------- | ----------- |
| `type`  | `string` | Class name of the associated model. |
| `uuid`  | `uuid`   | UUID of the associated model. |
| `title` | `string` | Title or name of the associated model. |

### `participants[]` – Participant

| Field   | Type     | Description |
| ------- | -------- | ----------- |
| `uuid`  | `uuid`   | Participant's UUID. |
| `name`  | `string` | Participant's name. |
| `email` | `string` | Participant's email. |
| `pivot` | `object` | Many-to-many relationship information. |

### `pivot` – Participant Information in Conversation

| Field             | Type      | Description |
| ----------------- | --------- | ----------- |
| `conversation_id` | `integer` | Internal conversation ID. |
| `user_id`         | `integer` | Internal user ID. |
| `unread_count`    | `integer` | Number of unread messages. |
| `last_read_at`    | `string`  | Date and time of last read (ISO-8601 format). |
| `joined_at`       | `string`  | Date and time when the participant joined the conversation (ISO-8601 format). |
| `created_at`      | `string`  | Relationship creation date (ISO-8601 format). |
| `updated_at`      | `string`  | Last update date of the relationship (ISO-8601 format). |

---

## HTTP Status

- 200: OK
- 401: Unauthorized
- 500: Internal Server Error

---

## Notes

* Conversations are ordered by last message date (`last_message_at`) in descending order.
* Only conversations where the authenticated user is a participant are returned.
* The `taggable_info` field can be `null` if there is no context associated with the conversation.
* The `unread_count` counter indicates how many messages the participant has not read yet.

---

## Related

- [Create Conversation](./ChatConversationsCreate.md)
- [List Conversation Messages](./ChatConversationMessages.md)

---

## Changelog

- 2025-10-15: Initial documentation creation.
