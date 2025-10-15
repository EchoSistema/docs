# Chat – Send Message

## Endpoint

`POST /api/v1/chat/messages`

Sends a new message in an existing conversation, with support for different content types, attachments, and device information.

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

### Request Body Parameters

| Parameter               | Type     | Required | Description |
| ----------------------- | -------- | -------- | ----------- |
| `conversation_uuid`     | `uuid`   | Yes      | Conversation UUID. |
| `receiver_uuid`         | `uuid`   | Yes      | Message receiver's UUID. |
| `context_text`          | `string` | Yes      | Message text (max. 5000 characters). |
| `context_type`          | `string` | Yes      | Content type: `text`, `image`, `file`, `audio`, `video`. |
| `attachments[]`         | `array`  | No       | List of attachments (free structure). |
| `metadata`              | `object` | No       | Additional metadata (free structure). |
| `device.type`           | `string` | Yes      | Device type: `mobile`, `web`, `desktop`. |
| `device.os`             | `string` | Yes      | Operating system (e.g., `Windows`, `iOS`, `Android`). |
| `device.os_version`     | `string` | No       | Operating system version. |
| `device.app_version`    | `string` | No       | Application version. |
| `device.model`          | `string` | No       | Device model. |
| `device.browser`        | `string` | No       | Browser name. |
| `device.browser_version`| `string` | No       | Browser version. |

---

## Examples

### Request Example (curl)

```bash
curl -X POST \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Content-Type: application/json" \
  -d '{
    "conversation_uuid": "9b732d6a-2e56-4db8-9b2e-6fd8c14e7f28",
    "receiver_uuid": "6b420a39-0b23-3a95-6a0b-3ca5900b4c05",
    "context_text": "Hello, how are you?",
    "context_type": "text",
    "attachments": [],
    "metadata": {
      "source": "web_app"
    },
    "device": {
      "type": "web",
      "os": "Windows",
      "os_version": "10",
      "app_version": "1.0.0",
      "browser": "Chrome",
      "browser_version": "120.0"
    }
  }' \
  "https://sandbox.your-domain.com/api/v1/chat/messages"
```

### Request Body Example

```json
{
  "conversation_uuid": "9b732d6a-2e56-4db8-9b2e-6fd8c14e7f28",
  "receiver_uuid": "6b420a39-0b23-3a95-6a0b-3ca5900b4c05",
  "context_text": "Hello, how are you?",
  "context_type": "text",
  "attachments": [],
  "metadata": {
    "source": "web_app"
  },
  "device": {
    "type": "web",
    "os": "Windows",
    "os_version": "10",
    "app_version": "1.0.0",
    "browser": "Chrome",
    "browser_version": "120.0"
  }
}
```

### Response Example

```json
{
  "message": "Message sent successfully",
  "data": {
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
    "metadata": {
      "source": "web_app"
    },
    "reactions": [],
    "status": {
      "is_read": false,
      "read_at": null,
      "delivered_at": "2025-10-15T10:30:00+00:00",
      "is_deleted": false
    },
    "timestamps": {
      "created_at": "2025-10-15T10:30:00+00:00",
      "updated_at": "2025-10-15T10:30:00+00:00"
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
}
```

---

## JSON Structure Explained

### Success Response

| Field     | Type     | Description |
| --------- | -------- | ----------- |
| `message` | `string` | Confirmation message. |
| `data`    | `object` | Created message object. |

### `data` – Message Object

See complete structure in [List Conversation Messages](./ChatConversationMessages.md).

---

## HTTP Status

- 201: Created successfully
- 400: Bad Request
- 401: Unauthorized
- 403: Forbidden (user is not a conversation participant)
- 404: Conversation or receiver not found
- 422: Validation error
- 500: Internal Server Error

---

## Errors

### Validation Error Example

```json
{
  "message": "The conversation uuid field is required.",
  "errors": {
    "conversation_uuid": [
      "The conversation uuid field is required."
    ],
    "context_text": [
      "The context text must not be greater than 5000 characters."
    ]
  }
}
```

### Internal Error Example

```json
{
  "success": false,
  "message": "Failed to send message",
  "error": "Database connection error"
}
```

---

## Notes

* The authenticated user must be a participant in the specified conversation.
* Message text is limited to 5000 characters.
* The message is created with status `is_read: false` and `read_at: null` by default.
* The `delivered_at` field is automatically populated with the current date/time.
* The conversation is updated with `last_message_text` and `last_message_at` after sending.
* A `MessageSent` event is broadcast to notify other participants in real-time.
* Device fields (`device`) are stored for analytics and statistics.

---

## Related

- [List Conversations](./ChatConversationsList.md)
- [Create Conversation](./ChatConversationsCreate.md)
- [List Conversation Messages](./ChatConversationMessages.md)
- [Mark Message as Read](./ChatMessageMarkRead.md)
- [Device Statistics](./ChatDeviceStats.md)

---

## Changelog

- 2025-10-15: Initial documentation creation.
