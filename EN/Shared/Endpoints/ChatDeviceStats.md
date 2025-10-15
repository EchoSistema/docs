# Chat – Device Statistics

## Endpoint

`GET /api/v1/chat/stats/devices`

Retrieves aggregated statistics about device types used to send messages in the authenticated user's conversations.

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

## Examples

### Request Example (curl)

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  "https://sandbox.your-domain.com/api/v1/chat/stats/devices"
```

### Response Example

```json
{
  "data": [
    {
      "_id": "web",
      "count": 125
    },
    {
      "_id": "mobile",
      "count": 87
    },
    {
      "_id": "desktop",
      "count": 43
    }
  ]
}
```

---

## JSON Structure Explained

### Success Response

| Field    | Type    | Description |
| -------- | ------- | ----------- |
| `data[]` | `array` | List of statistics aggregated by device type. |

### `data[]` – Device Statistic

| Field   | Type      | Description |
| ------- | --------- | ----------- |
| `_id`   | `string`  | Device type: `web`, `mobile`, `desktop`. |
| `count` | `integer` | Total number of messages sent from this device type. |

---

## HTTP Status

- 200: OK
- 401: Unauthorized
- 500: Internal Server Error

---

## Notes

* Statistics include only messages from conversations where the authenticated user is a participant.
* Aggregation is performed using the MongoDB Aggregation Framework.
* The `_id` field corresponds to the `device.type` field stored in each message.
* If the user has no conversations, an empty array is returned.
* This API is useful for usage and behavior analytics, allowing identification of which devices are most commonly used.
* The data can be used to optimize user experience across different platforms.

---

## Related

- [List Conversations](./ChatConversationsList.md)
- [Send Message](./ChatMessageSend.md)
- [List Conversation Messages](./ChatConversationMessages.md)

---

## Changelog

- 2025-10-15: Initial documentation creation.
