# Chat – List Conversation Messages

## Endpoint

`GET /api/v1/chat/conversations/{uuid}/messages`

Retrieves all messages from a specific conversation, including conversation information and its associated context.

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

### Path Parameters

| Parameter | Type   | Required | Description |
| --------- | ------ | -------- | ----------- |
| `uuid`    | `uuid` | Yes      | Conversation UUID. |

---

## Examples

### Request Example (curl)

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  "https://sandbox.your-domain.com/api/v1/chat/conversations/9b732d6a-2e56-4db8-9b2e-6fd8c14e7f28/messages"
```

### Response Example

```json
{
  "data": {
    "conversation": {
      "uuid": "9b732d6a-2e56-4db8-9b2e-6fd8c14e7f28",
      "taggable_type": "App\\Models\\Order",
      "last_message_text": "Hello, how are you?",
      "last_message_at": "2025-10-15T10:30:00.000000Z",
      "is_archived": false,
      "created_at": "2025-10-01T08:00:00.000000Z",
      "updated_at": "2025-10-15T10:30:00.000000Z",
      "taggable_info": {
        "type": "Order",
        "uuid": "8a621c5b-1d45-4cb7-8a1d-5ec7b13d6e17",
        "title": "Order #1234"
      }
    },
    "messages": [
      {
        "message_id": "670e3a1b8f9c2d001e4f5a6b",
        "conversation_uuid": "9b732d6a-2e56-4db8-9b2e-6fd8c14e7f28",
        "sender": {
          "uuid": "7c531b4a-1c34-4ba6-7b0c-4db6a12c5d16",
          "name": "John Smith",
          "avatar": "https://cdn.example.com/avatars/john.jpg"
        },
        "receiver": {
          "uuid": "6b420a39-0b23-3a95-6a0b-3ca5900b4c05",
          "name": "Mary Johnson",
          "avatar": "https://cdn.example.com/avatars/mary.jpg"
        },
        "context": {
          "text": "Hello, how are you?",
          "type": "text"
        },
        "attachments": [],
        "metadata": {},
        "reactions": [
          {
            "user_uuid": "6b420a39-0b23-3a95-6a0b-3ca5900b4c05",
            "emoji": "👍",
            "created_at": "2025-10-15T10:31:00+00:00"
          }
        ],
        "status": {
          "is_read": true,
          "read_at": "2025-10-15T10:31:00+00:00",
          "delivered_at": "2025-10-15T10:30:00+00:00",
          "is_deleted": false
        },
        "timestamps": {
          "created_at": "2025-10-15T10:30:00+00:00",
          "updated_at": "2025-10-15T10:31:00+00:00"
        },
        "pbk": "platform_public_key",
        "device": {
          "type": "web",
          "os": "Windows",
          "os_version": "10",
          "app_version": "1.0.0",
          "model": null,
          "browser": "Chrome",
          "browser_version": "120.0"
        }
      }
    ]
  }
}
```

---

## JSON Structure Explained

### Success Response

| Field  | Type     | Description |
| ------ | -------- | ----------- |
| `data` | `object` | Contains the conversation and its messages. |

### `data.conversation` – Conversation Object

| Field               | Type      | Description |
| ------------------- | --------- | ----------- |
| `uuid`              | `uuid`    | Unique identifier for the conversation. |
| `taggable_type`     | `string`  | Type of associated polymorphic model. |
| `last_message_text` | `string`  | Text of the last message. |
| `last_message_at`   | `string`  | Date of last message (ISO-8601 format). |
| `is_archived`       | `boolean` | Indicates whether the conversation is archived. |
| `created_at`        | `string`  | Conversation creation date (ISO-8601 format). |
| `updated_at`        | `string`  | Last update date (ISO-8601 format). |
| `taggable_info`     | `object`  | Information about associated context. |

### `data.messages[]` – Messages Array

| Field                | Type     | Description |
| -------------------- | -------- | ----------- |
| `message_id`         | `string` | Unique message ID (MongoDB ObjectId). |
| `conversation_uuid`  | `uuid`   | Conversation UUID. |
| `sender`             | `object` | Sender information. |
| `receiver`           | `object` | Receiver information. |
| `context`            | `object` | Message content. |
| `attachments[]`      | `array`  | List of attachments (files, images, etc.). |
| `metadata`           | `object` | Additional metadata. |
| `reactions[]`        | `array`  | List of reactions (emojis). |
| `status`             | `object` | Delivery and read status. |
| `timestamps`         | `object` | Creation and update timestamps. |
| `pbk`                | `string` | Platform public key. |
| `device`             | `object` | Sending device information. |

### `sender` / `receiver` – User Information

| Field    | Type     | Description |
| -------- | -------- | ----------- |
| `uuid`   | `uuid`   | User UUID. |
| `name`   | `string` | User name. |
| `avatar` | `string` | Avatar URL (can be null). |

### `context` – Message Content

| Field  | Type     | Description |
| ------ | -------- | ----------- |
| `text` | `string` | Message text. |
| `type` | `string` | Content type: `text`, `image`, `file`, `audio`, `video`. |

### `reactions[]` – Reactions

| Field        | Type     | Description |
| ------------ | -------- | ----------- |
| `user_uuid`  | `uuid`   | UUID of user who reacted. |
| `emoji`      | `string` | Reaction emoji. |
| `created_at` | `string` | Reaction date (ISO-8601 format). |

### `status` – Message Status

| Field          | Type      | Description |
| -------------- | --------- | ----------- |
| `is_read`      | `boolean` | Indicates if the message was read. |
| `read_at`      | `string`  | Read date (ISO-8601 format, null if unread). |
| `delivered_at` | `string`  | Delivery date (ISO-8601 format). |
| `is_deleted`   | `boolean` | Indicates if the message was deleted. |

### `device` – Device Information

| Field             | Type     | Description |
| ----------------- | -------- | ----------- |
| `type`            | `string` | Device type: `mobile`, `web`, `desktop`. |
| `os`              | `string` | Operating system. |
| `os_version`      | `string` | Operating system version (can be null). |
| `app_version`     | `string` | Application version (can be null). |
| `model`           | `string` | Device model (can be null). |
| `browser`         | `string` | Browser (can be null). |
| `browser_version` | `string` | Browser version (can be null). |

---

## HTTP Status

- 200: OK
- 401: Unauthorized
- 403: Forbidden (user is not a conversation participant)
- 404: Conversation not found
- 500: Internal Server Error

---

## Notes

* Only conversation participants can view its messages.
* Messages are ordered by creation date in ascending order (oldest first).
* The `taggable_info` field provides context about what originated the conversation (e.g., an order, a booking, etc.).
* Deleted messages (`is_deleted: true`) are still returned but can be hidden on the frontend.

---

## Related

- [List Conversations](./ChatConversationsList.md)
- [Send Message](./ChatMessageSend.md)
- [Mark Message as Read](./ChatMessageMarkRead.md)
- [Add Reaction](./ChatMessageAddReaction.md)

---

## Changelog

- 2025-10-15: Initial documentation creation.
