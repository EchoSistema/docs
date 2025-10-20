# Shared – Show Contact Message

## Endpoint

`GET /api/v1/contacts/{message:uuid}`

## Authentication

Requires Bearer token and platform header.

## Headers

| Header | Type | Required | Description |
| ------ | ---- | -------- | ----------- |
| Authorization | `string` | Yes | `Bearer {token}`. |
| X-PUBLIC-KEY  | `string` | Yes | Platform public key. |
| Accept-Language | `string` | No | Locale for responses. |

## Parameters

Get details of a specific contact message. Permissions: `show.platform_message` or `show.all`, except when the authenticated user is the author.

## Examples

### Request example (curl)

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: en" \
  "https://sandbox.your-domain.com/api/v1/contacts/{message:uuid}"
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
    "ip_address": "200.200.200.5",
    "images": [ { "uuid": "a1dcba3c-4c74-11ef-92b0-acde48001122", "url": "https://cdn.example.com/platform/contacts/a1dcba3c.png", "usage": "platform_contact" } ],
    "user": { "uuid": "1fdf0b1e-2cde-4b12-93f5-02d41a0f38c4", "name": "Maria Silva", "email": "maria.silva@example.com" },
    "platform": { "uuid": "7a3b6d12-4d9f-4f5c-b41b-6ce39e04d57f", "name": "Echosistema" },
    "first_reader": { "uuid": "f39af7fa-6622-4c46-ba0d-fefb699b10f8", "name": "John Admin", "email": "john.admin@example.com" }
  }
}
```

## JSON Structure Explained

| Field | Type | Description |
| ----- | ---- | ----------- |
| data  | `object` | Message resource. |

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

- Get details of a specific contact message

## Related

- See other Shared API endpoints

## Changelog

- 2025-10-16: Initial documentation
