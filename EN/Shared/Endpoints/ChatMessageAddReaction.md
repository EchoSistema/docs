# Chat – Add Reaction to Message

## Endpoint

`POST /api/v1/chat/messages/{messageId}/reactions`

Adds a reaction (emoji) to an existing message.

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

| Parameter   | Type     | Required | Description |
| ----------- | -------- | -------- | ----------- |
| `messageId` | `string` | Yes      | Message ID (MongoDB ObjectId). |

### Request Body Parameters

| Parameter | Type     | Required | Description |
| --------- | -------- | -------- | ----------- |
| `emoji`   | `string` | Yes      | Reaction emoji (max. 10 characters). |

---

## Examples

### Request Example (curl)

```bash
curl -X POST \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Content-Type: application/json" \
  -d '{
    "emoji": "👍"
  }' \
  "https://sandbox.your-domain.com/api/v1/chat/messages/670e3a1b8f9c2d001e4f5a6b/reactions"
```

### Request Body Example

```json
{
  "emoji": "👍"
}
```

### Response Example

```json
{
  "message": "Reaction added successfully"
}
```

---

## JSON Structure Explained

### Success Response

| Field     | Type     | Description |
| --------- | -------- | ----------- |
| `message` | `string` | Confirmation message. |

---

## HTTP Status

- 200: OK
- 400: Bad Request
- 401: Unauthorized
- 404: Message not found
- 422: Validation error (invalid or too long emoji)
- 500: Internal Server Error

---

## Errors

### Validation Error Example

```json
{
  "message": "The emoji field is required.",
  "errors": {
    "emoji": [
      "The emoji field is required."
    ]
  }
}
```

---

## Notes

* Any authenticated user can add a reaction to a message.
* The same user can add multiple reactions (different emojis) to the same message.
* The reaction is added to the message's `reactions` array with the user's UUID, emoji, and current date/time.
* The message's `timestamps.updated_at` field is updated.
* There is no validation if the user has already added the same emoji previously (allowing duplicates).
* The emoji is limited to 10 characters to support compound emojis.

---

## Related

- [List Conversation Messages](./ChatConversationMessages.md)
- [Send Message](./ChatMessageSend.md)
- [Mark Message as Read](./ChatMessageMarkRead.md)

---

## Changelog

- 2025-10-15: Initial documentation creation.
