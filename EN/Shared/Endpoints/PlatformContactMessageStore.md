# Shared – Create Contact Message

## Endpoint

`POST /api/v1/contact`

## Authentication

None (public endpoint). Requires platform header.

## Headers

| Header | Type | Required | Description |
| ------ | ---- | -------- | ----------- |
| X-PUBLIC-KEY | `string` | Yes | Platform public key (also accepts `public_key` in query). |
| X-APP-NAME | `string` | No | When present (and matching a platform name), reCAPTCHA is not required. |
| Accept-Language | `string` | No | Locale for validation messages (`pt-BR`, `en`, `es`, `gn`). |

## Parameters

Submit a contact form message

### Body (application/json)

| Field | Type | Required | Description |
| ----- | ---- | -------- | ----------- |
| `type` | `enum` | Yes | One of: `default`, `denunciation`, `report`. |
| `language` | `enum` | Yes | `pt-BR`, `en`, `es`, `gn`. |
| `g_recaptcha` | `string` | Required unless `X-APP-NAME` | reCAPTCHA token validated server-side. |
| `email` | `email` | Required without `phone` | Contact email. |
| `phone` | `string` | Required without `email` | Contact phone. |
| `content` | `string` | Yes | Message (max 5000 chars). Aliased from `message` if provided. |
| `name` | `string` | No | Sender name. |
| `user_uuid` | `uuid` | No | Associates to an existing user. |
| `anonymous` | `boolean` | No | When `true`, uses `anonymous@pigeons.email`. |

## Examples

### Request example (curl)

```bash
curl -X POST \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: en" \
  -H "Content-Type: application/json" \
  "https://sandbox.your-domain.com/api/v1/contact" \
  -d '{
    "type": "default",
    "language": "en",
    "g_recaptcha": "<token>",
    "email": "maria@example.com",
    "content": "I would like to know more about plans.",
    "name": "Maria Silva"
  }'
```

### Success `201 Created`

```json
{
  "success": true,
  "data": { "uuid": "2c3f9d26-4c68-4478-8d1f-9a1ea3b6b32e" }
}
```

## JSON Structure Explained

| Field | Type | Description |
| ----- | ---- | ----------- |
| success | `boolean` | Always `true` on success. |
| data | `object` | Response data. |

## HTTP Status

- 201: Created
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

- Submit a contact form message

## Related

- See other Shared API endpoints

## Changelog

- 2025-10-16: Initial documentation
