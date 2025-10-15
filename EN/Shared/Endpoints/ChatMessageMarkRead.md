# Chat – Mark Message as Read

## Endpoint

`PATCH /api/v1/chat/messages/{messageId}/read`

Marks a message as read, updating its status and recording the read date/time.

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

---

## Examples

### Request Example (curl)

```bash
curl -X PATCH \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  "https://sandbox.your-domain.com/api/v1/chat/messages/670e3a1b8f9c2d001e4f5a6b/read"
```

### Response Example

```json
{
  "message": "Message marked as read"
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
- 401: Unauthorized
- 403: Forbidden (user is not the message receiver)
- 404: Message not found
- 500: Internal Server Error

---

## Errors

### Authorization Error Example

```json
{
  "message": "Unauthorized"
}
```

---

## Notes

* Only the message receiver (`receiver`) can mark it as read.
* The operation updates the `status.is_read` field to `true` and `status.read_at` with the current date/time.
* The `timestamps.updated_at` field is also updated.
* If the message is already marked as read, the operation will still return success.
* This action can be used to update unread message counters in the user interface.

---

## Related

- [List Conversation Messages](./ChatConversationMessages.md)
- [Send Message](./ChatMessageSend.md)
- [Add Reaction](./ChatMessageAddReaction.md)

---

## Changelog

- 2025-10-15: Initial documentation creation.
