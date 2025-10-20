# Shared – List Contact Messages

## Endpoint

`GET /api/v1/contacts`

## Authentication

Requires Bearer token and platform header.

## Headers

| Header | Type | Required | Description |
| ------ | ---- | -------- | ----------- |
| Authorization | `string` | Yes | `Bearer {token}`. |
| X-PUBLIC-KEY  | `string` | Yes | Platform public key. |
| Accept-Language | `string` | No | Locale for responses. |

## Parameters

List contact messages with filters and pagination. Permissions: `index.platform_message` or `index.all`. Non-backoffice users are restricted to their platform; guests see only their own messages.

### Query parameters

| Param | Type | Description |
| ----- | ---- | ----------- |
| `page` | `integer` | Page number (default 1). |
| `per_page` | `integer` | Items per page (default 25). |
| `no_paginate` | `boolean` | When true, disables pagination. |
| `limit` | `integer` | Limit when `no_paginate` is true. |
| `sort_by` | `string` | Sort column (default `created_at`). |
| `direction` | `string` | `ASC`/`DESC` (default `DESC`). |
| `uuid` | `uuid` | Exact identifier. |
| `email` | `email` | Contact email. |
| `phone` | `string` | Phone provided. |
| `name` | `string` | Sender name (like search). |
| `type` | `string` | `default`, `denunciation`, `report`. |
| `message_lang` | `string` | `pt-BR`, `en`, `es`, `gn`. |
| `created_at` | `date` | Exact date (`YYYY-MM-DD`). |
| `interval` | `string` | `hour`, `day`, `week`, `month`, `year`. |
| `period.from` / `period.to` | `date` | Custom date range. |
| `user` | `string` | ID, UUID, email, or part of the author name. |
| `platform` | `string` | ID, UUID, slug, or part of the platform name. |
| `ip_address` | `ipv4` | Sender IP. |
| `ip_banned` | `boolean` | Only banned IPs. |
| `is_robot` | `boolean` | Only IPs identified as bots. |
| `is_read` | `boolean` | Filter by read status. |
| `first_reader` | `string` | First reader identifier. |
| `content` | `string` | Full-text search over message body. |
| `with_images` | `boolean` | Only messages with attachments. |

## Examples

### Request example (curl)

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: en" \
  "https://sandbox.your-domain.com/api/v1/contacts"
```

### Response example (200 OK)

```json
{
  "data": [
    {
      "uuid": "2c3f9d26-4c68-4478-8d1f-9a1ea3b6b32e",
      "name": "Maria Silva",
      "is_read": false,
      "email": "maria.silva@example.com",
      "created_at": "2024-06-06T18:12:45+00:00",
      "user": { "uuid": "1fdf0b1e-2cde-4b12-93f5-02d41a0f38c4", "name": "Maria Silva", "email": "maria.silva@example.com" },
      "content": "I'd like to learn more about the plans..."
    }
  ],
  "links": { "first": "https://api.example.com/api/v1/contacts?page=1", "last": "https://api.example.com/api/v1/contacts?page=5", "prev": null, "next": "https://api.example.com/api/v1/contacts?page=2" },
  "meta": { "current_page": 1, "from": 1, "last_page": 5, "path": "https://api.example.com/api/v1/contacts", "per_page": 25, "to": 25, "total": 112 }
}
```

## JSON Structure Explained

| Field | Type | Description |
| ----- | ---- | ----------- |
| data  | `array` | List of messages. Pagination metadata is included when applicable. |

## HTTP Status

- 200: OK
- 400: Bad Request
- 401: Unauthorized
- 403: Forbidden
- 404: Not Found
- 422: Unprocessable Entity
- 429: Too Many Requests
- 500: Internal Server Error

## Errors

```json
{
  "message": "Error message"
}
```

## Notes

- List all contact messages (with filters)

## Related

- See other Shared API endpoints

## Changelog

- 2025-10-16: Initial documentation
