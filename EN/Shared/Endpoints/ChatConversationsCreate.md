# Chat – Create Conversation

## Endpoint

`POST /api/v1/chat/conversations`

Creates a new conversation between the authenticated user and another participant.

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

| Parameter          | Type      | Required | Description |
| ------------------ | --------- | -------- | ----------- |
| `participant_uuid` | `uuid`    | Yes      | UUID of the participant to be added to the conversation. |
| `taggable_type`    | `string`  | No       | Type of polymorphic model to be associated with the conversation (e.g., `App\Models\Order`). |
| `taggable_id`      | `integer` | No       | ID of the polymorphic model to be associated with the conversation. |

---

## Examples

### Request Example (curl)

```bash
curl -X POST \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Content-Type: application/json" \
  -d '{
    "participant_uuid": "6b420a39-0b23-3a95-6a0b-3ca5900b4c05",
    "taggable_type": "App\\Models\\Order",
    "taggable_id": 123
  }' \
  "https://sandbox.your-domain.com/api/v1/chat/conversations"
```

### Request Body Example

```json
{
  "participant_uuid": "6b420a39-0b23-3a95-6a0b-3ca5900b4c05",
  "taggable_type": "App\\Models\\Order",
  "taggable_id": 123
}
```

### Response Example

```json
{
  "message": "Conversation created successfully",
  "data": {
    "uuid": "9b732d6a-2e56-4db8-9b2e-6fd8c14e7f28",
    "taggable_type": "App\\Models\\Order",
    "taggable_id": 123,
    "last_message_text": null,
    "last_message_at": null,
    "is_archived": false,
    "created_at": "2025-10-15T10:30:00.000000Z",
    "updated_at": "2025-10-15T10:30:00.000000Z",
    "participants": [
      {
        "uuid": "7c531b4a-1c34-4ba6-7b0c-4db6a12c5d16",
        "name": "John Smith",
        "email": "john@example.com",
        "pivot": {
          "conversation_id": 1,
          "user_id": 10,
          "unread_count": 0,
          "last_read_at": null,
          "joined_at": "2025-10-15T10:30:00.000000Z",
          "created_at": "2025-10-15T10:30:00.000000Z",
          "updated_at": "2025-10-15T10:30:00.000000Z"
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
          "last_read_at": null,
          "joined_at": "2025-10-15T10:30:00.000000Z",
          "created_at": "2025-10-15T10:30:00.000000Z",
          "updated_at": "2025-10-15T10:30:00.000000Z"
        }
      }
    ]
  }
}
```

---

## JSON Structure Explained

### Success Response

| Field     | Type     | Description |
| --------- | -------- | ----------- |
| `message` | `string` | Confirmation message. |
| `data`    | `object` | Created conversation object with loaded participants. |

### `data` – Conversation Object

| Field               | Type      | Description |
| ------------------- | --------- | ----------- |
| `uuid`              | `uuid`    | Unique identifier for the conversation. |
| `taggable_type`     | `string`  | Type of associated polymorphic model (when applicable). |
| `taggable_id`       | `integer` | ID of associated polymorphic model (when applicable). |
| `last_message_text` | `string`  | Text of the last message (null in new conversations). |
| `last_message_at`   | `string`  | Date of last message (null in new conversations). |
| `is_archived`       | `boolean` | Indicates whether the conversation is archived. |
| `created_at`        | `string`  | Conversation creation date (ISO-8601 format). |
| `updated_at`        | `string`  | Last update date (ISO-8601 format). |
| `participants[]`    | `array`   | List of conversation participants. |

---

## HTTP Status

- 201: Created successfully
- 400: Bad Request
- 401: Unauthorized
- 422: Validation error (participant not found or invalid parameters)
- 500: Internal Server Error

---

## Errors

### Validation Error Example

```json
{
  "message": "The participant uuid field is required.",
  "errors": {
    "participant_uuid": [
      "The participant uuid field is required."
    ]
  }
}
```

### Internal Error Example

```json
{
  "success": false,
  "message": "Failed to create conversation",
  "error": "Database connection error"
}
```

---

## Notes

* The conversation is created with both participants (authenticated user and specified participant).
* The `taggable_type` and `taggable_id` fields are optional and allow associating the conversation with another model (e.g., order, booking, etc.).
* The conversation is created with `is_archived` set to `false` by default.
* Both participants have `unread_count` initialized to `0`.
* The join date (`joined_at`) for both participants is set at the time of creation.

---

## Related

- [List Conversations](./ChatConversationsList.md)
- [Send Message](./ChatMessageSend.md)
- [List Conversation Messages](./ChatConversationMessages.md)

---

## Changelog

- 2025-10-15: Initial documentation creation.
