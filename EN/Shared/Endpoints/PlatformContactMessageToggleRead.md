# Shared – Toggle Contact Message Read Status

## Endpoint

`PATCH /api/v1/contacts/{message:uuid}/toggle-read`

## Authentication

Requires Bearer token and platform header.

## Headers

| Header | Type | Required | Description |
| ------ | ---- | -------- | ----------- |
| Authorization | `string` | Yes | `Bearer {token}`. |
| X-PUBLIC-KEY  | `string` | Yes | Platform public key. |
| Accept-Language | `string` | No | Locale for responses. |

## Parameters

Toggle the read status of a contact message. Permissions: `show.platform_message` or `show.all`, except when the authenticated user is the author. Non-backoffice users can only toggle messages from their platform.

## Examples

### Request example (curl)

```bash
curl -X PATCH \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: en" \
  "https://sandbox.your-domain.com/api/v1/contacts/{message:uuid}/toggle-read"
```

### Success `200 OK`

```json
{
  "data": {
    "uuid": "2c3f9d26-4c68-4478-8d1f-9a1ea3b6b32e",
    "name": "Maria Silva",
    "is_read": true,
    "email": "maria.silva@example.com",
    "created_at": "2024-06-06T18:12:45+00:00",
    "type": "default",
    "language": "en",
    "phone": "+55 11912345678",
    "content": "I'd like to learn more about the plans and available integrations...",
    "ip_address": "200.200.200.5"
  }
}
```

## JSON Structure Explained

| Field | Type | Description |
| ----- | ---- | ----------- |
| data  | `object` | Message resource (same as detail endpoint). |

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

- Toggle the read status of a contact message

## Related

- See other Shared API endpoints

## Changelog

- 2025-10-16: Initial documentation
