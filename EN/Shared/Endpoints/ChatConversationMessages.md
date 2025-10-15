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
  "data": [
    {
      "conversation_uuid": "361be1bb-d7fb-4c3a-8a02-9071f69bacc8",
      "sender": {
        "uuid": "c1f1f474-4346-3118-83c5-2b5a6e6aff48",
        "name": "Ewerton Girardi",
        "avatar": "https://storage.echosistema.online/main/images/learn/forensic-academy/avatar/ewerton-daniel-080933300-1643321172.png"
      },
      "receiver": {
        "uuid": "05e12689-1a76-3af3-8326-7028e3b8a503",
        "name": "Daniel C. S. C. Soares",
        "avatar": "https://storage.echosistema.online/main/images/learn/forensic-academy/avatar/3-daniel-c-s-c-soares.png"
      },
      "context": {
        "text": "Hello! How are you?",
        "texts": {
          "pt-BR": "Olá! Como você está?",
          "en": "Hello! How are you?"
        },
        "type": "text"
      },
      "attachments": [],
      "metadata": [],
      "reactions": [],
      "status": {
        "is_read": true,
        "read_at": "2025-10-15T09:36:04-03:00",
        "delivered_at": "2025-10-15T09:35:04-03:00",
        "is_deleted": false
      },
      "timestamps": {
        "created_at": "2025-10-15T09:34:04-03:00",
        "updated_at": "2025-10-15T09:34:04-03:00"
      },
      "pbk": "1396fd3b-d56b-362f-8ebb-c068bb910b69",
      "device": {
        "type": "mobile",
        "os": "macOS",
        "os_version": "11",
        "app_version": "1.0.0",
        "model": null,
        "browser": "Edge",
        "browser_version": "121.0"
      },
      "message_id": "68efa2cc35334e074c00abde",
      "id": "68efa2cc35334e074c00abde"
    }
  ],
  "conversation": {
    "uuid": "361be1bb-d7fb-4c3a-8a02-9071f69bacc8",
    "taggable_type": null,
    "last_message_text": "Thanks! Once I finish I’ll notify QA.",
      "last_message_at": "2025-10-15T13:29:04.000000Z",
      "is_archived": false,
      "created_at": "2025-10-15T13:34:04.000000Z",
      "updated_at": "2025-10-15T13:34:04.000000Z",
      "taggable_info": null
  }
}
```

---

## JSON Structure Explained

### Response Body

| Field          | Type     | Description |
| -------------- | -------- | ----------- |
| `data`         | `array`  | Message entries belonging to the conversation. |
| `conversation` | `object` | Conversation context for the listed messages. |

### `conversation`

| Field               | Type      | Description |
| ------------------- | --------- | ----------- |
| `uuid`              | `uuid`    | Unique identifier for the conversation. |
| `taggable_type`     | `string`  | Type of associated polymorphic model. |
| `last_message_text` | `string`  | Text of the last message. |
| `last_message_at`   | `string`  | Date of the last message (ISO-8601 format). |
| `is_archived`       | `boolean` | Indicates whether the conversation is archived. |
| `created_at`        | `string`  | Conversation creation date (ISO-8601 format). |
| `updated_at`        | `string`  | Last update date (ISO-8601 format). |
| `taggable_info`     | `object`  | Contextual information about the conversation (may be `null`). |

### `data[]`

| Field               | Type     | Description |
| ------------------- | -------- | ----------- |
| `message_id`        | `string` | Unique message ID (MongoDB ObjectId). |
| `id`                | `string` | Legacy alias for `message_id`. |
| `conversation_uuid` | `uuid`   | Conversation UUID. |
| `sender`            | `object` | Sender information. |
| `receiver`          | `object` | Receiver information. |
| `context`           | `object` | Message content payload. |
| `attachments`       | `array`  | Attachment list (can be empty). |
| `metadata`          | `array`  | Additional metadata entries. |
| `reactions`         | `array`  | Reaction entries (emojis). |
| `status`            | `object` | Delivery/read status flags. |
| `timestamps`        | `object` | Creation and update timestamps. |
| `pbk`               | `string` | Platform public key tied to the message. |
| `device`            | `object` | Sending device information. |

### `sender` / `receiver` – User Information

| Field    | Type     | Description |
| -------- | -------- | ----------- |
| `uuid`   | `uuid`   | User UUID. |
| `name`   | `string` | User name. |
| `avatar` | `string` | Avatar URL (can be null). |

### `context` – Message Content

| Field  | Type     | Description |
| ------ | -------- | ----------- |
| `text`  | `string`  | Primary message text. |
| `texts` | `object?` | Optional translations keyed by locale. |
| `type`  | `string`  | Content type: `text`, `image`, `file`, `audio`, `video`. |

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
* The `context.texts` field is optional and only appears when translations are stored for the message.

---

## Related

- [List Conversations](./ChatConversationsList.md)
- [Send Message](./ChatMessageSend.md)
- [Mark Message as Read](./ChatMessageMarkRead.md)
- [Add Reaction](./ChatMessageAddReaction.md)

---

## Changelog

- 2025-10-15: Initial documentation creation.
