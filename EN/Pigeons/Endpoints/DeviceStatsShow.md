# Pigeons – Get Device Statistics

## Endpoint
```
GET /api/v1/pigeons/chat/stats/devices
```

## Authentication
Required – Bearer token with `backoffice` ability.

## Headers
| Header | Type | Required | Description |
|--------|------|----------|-------------|
| `Authorization` | string | Yes | Bearer {token} |
| `X-PUBLIC-KEY` | string | Yes | Platform public key |
| `Accept-Language` | string | No | Locale for translatable fields (pt-BR, en, es) |

## Parameters
No parameters required.

## Examples

### Request
```bash
curl -X GET \
  https://api.example.com/api/v1/pigeons/chat/stats/devices \
  -H 'Authorization: Bearer {token}' \
  -H 'X-PUBLIC-KEY: {platform-key}' \
  -H 'Accept-Language: en'
```

### Response
```json
{
  "data": [
    {
      "_id": "mobile",
      "count": 150
    },
    {
      "_id": "web",
      "count": 85
    },
    {
      "_id": "desktop",
      "count": 23
    }
  ]
}
```

## Explained JSON Structure
| Field | Type | Description |
|-------|------|-------------|
| `data` | array | Array of device type statistics |
| `data[]._id` | string | Device type (mobile, web, desktop) |
| `data[].count` | integer | Number of messages sent from this device type |

## HTTP Status
| Code | Description |
|------|-------------|
| 200 | Success – statistics retrieved |
| 401 | Unauthorized – invalid or missing token |
| 403 | Forbidden – insufficient permissions |
| 429 | Too Many Requests – rate limit exceeded |
| 500 | Internal Server Error |

## Notes
- Returns aggregated statistics of messages by device type
- Only includes statistics from conversations where the authenticated user is a participant
- Data is aggregated from MongoDB using the `$group` aggregation pipeline
- If the user has no conversations, returns an empty array
- Device types are: `mobile`, `web`, or `desktop`
- The count represents the total number of messages sent from each device type across all user conversations

## Related
- [List Conversations](./ConversationsIndex.md)
- [Get Conversation Messages](./ConversationMessagesIndex.md)
- [Send Message](./MessagesStore.md)
