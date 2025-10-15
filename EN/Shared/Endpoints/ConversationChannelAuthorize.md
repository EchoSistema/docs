# Shared – Authorize conversation.{uuid} Channel

## Endpoint

```
POST /broadcasting/auth
```

## Authentication

Required – Bearer {token}

## Headers

| Header         | Type   | Required | Description |
| -------------- | ------ | -------- | ----------- |
| Authorization  | string | Yes      | `Bearer {token}` credential for the authenticated user. |
| X-PUBLIC-KEY   | string | No       | Only needed when the application also enforces a platform public key. |
| Accept-Language| string | No       | IETF locale (`pt-BR`, `en`, `es`) for translatable messages. |

## Parameters

### Path parameters

None.

### Query parameters

None.

### Request body

| Field         | Type   | Required | Description |
| ------------- | ------ | -------- | ----------- |
| channel_name  | string | Yes      | Must follow `conversation.{uuid}`, where `{uuid}` is the conversation identifier. |
| socket_id     | string | Yes      | Socket identifier provided by Pusher/Echo when the WebSocket is opened. |

```json
{
  "channel_name": "conversation.0f7d83cf-54b6-4ce0-9d33-8d6e9539d3c2",
  "socket_id": "1234.5678"
}
```

## Examples

### Request example (curl)

```bash
curl -X POST \
  -H "Authorization: Bearer <token>" \
  -H "Content-Type: application/json" \
  https://sandbox.example.com/broadcasting/auth \
  -d '{
    "channel_name": "conversation.0f7d83cf-54b6-4ce0-9d33-8d6e9539d3c2",
    "socket_id": "1234.5678"
  }'
```

### Response example

```json
{
  "auth": "123456:abcdef0123456789",
  "channel_data": {
    "user_id": 42,
    "user_info": {
      "uuid": "b7c1e1f2-8f20-4e4b-a6e1-0bf8f2c8c7b3"
    }
  }
}
```

## Explained JSON Structure

| Field                     | Type   | Description |
| ------------------------- | ------ | ----------- |
| auth                      | string | Backend-generated token that authorizes the private channel subscription. |
| channel_data              | object | Optional payload forwarded to the broadcast provider. |
| channel_data.user_id      | integer| Internal ID of the authenticated user. |
| channel_data.user_info    | object | Supplemental metadata about the user. |
| channel_data.user_info.uuid | string | UUID of the authenticated user. |

## HTTP Status

- 200: Authorization granted
- 401: Missing or invalid token
- 403: User is not a participant of the conversation
- 404: Conversation not found
- 429: Rate limit exceeded
- 500: Unexpected internal error

## Errors

```json
{
  "message": "You are not allowed to join this conversation.",
  "errors": null
}
```

## Notes

- Only participants of the conversation identified by the UUID can subscribe to `conversation.{uuid}`.
- Authorization reuses the `participants` relationship to validate membership atomically.
- Ensure the conversation UUID stays in sync between MySQL (`conversations` table) and the frontend client.

## Related

- No related endpoints are documented yet.

## Changelog

- 2025-10-15: Initial document.
