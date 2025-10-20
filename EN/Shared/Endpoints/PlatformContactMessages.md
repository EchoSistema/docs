# Platform - Contact Messages

## Overview

Endpoints responsible for the message flow submitted via the platform contact form. They allow the public contact to be registered (with optional reCAPTCHA validation for web, or `X-APP-NAME` header for mobile apps), attach images to the newly created ticket, and, inside the authenticated backoffice, list, view, and toggle messages as read.

## Endpoints

| Method | Path | Description | Authentication |
| ------ | ---- | ----------- | -------------- |
| `POST` | `/api/v1/contact` | Registers a new contact message. | No token required; `X-PUBLIC-KEY` header mandatory. |
| `POST` | `/api/v1/contact/{message}/images` | Adds an image linked to the message. | No token required; `X-PUBLIC-KEY` header mandatory. |
| `GET` | `/api/v1/contacts` | Lists messages with filters/pagination. | Requires `auth:sanctum` + `X-PUBLIC-KEY`. |
| `GET` | `/api/v1/contacts/{message}` | Shows full message details. | Requires `auth:sanctum` + `X-PUBLIC-KEY`. |
| `PATCH` | `/api/v1/contacts/{message}/toggle-read` | Toggles the message read status. | Requires `auth:sanctum` + `X-PUBLIC-KEY`. |

> **Backoffice Permissions**
> - Listing requires `index.platform_message` or `index.all` in the platform role.
> - Show/toggle requires `show.platform_message` or `show.all`, except when the user is the author.
> - Non-backoffice users only access messages for their own platform; guest users can only view their own submissions.

---

## Create message (`POST /api/v1/contact`)

### Headers

| Header | Type | Required | Description |
|--------|------|----------|-------------|
| `X-PUBLIC-KEY` | `string` | Yes | Platform public key (also accepts `public_key` in the query string). |
| `X-APP-NAME` | `string` | No | Application name. When provided, bypasses reCAPTCHA validation. |
| `Content-Type` | `application/json` | Yes | JSON body. |
| `Accept-Language` | `string` | No | Validation message locale (`pt-BR`, `en`, `es`, `gn`). |

### Body (`application/json`)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `type` | `enum` | Yes | `general`, `denunciation`, or `report`. |
| `language` | `enum` | Yes | Form language: `pt-BR`, `en`, `es`, `gn`. |
| `g_recaptcha` | `string` | Conditional | Token validated via `ReCaptchaRule`. Not required when `X-APP-NAME` header is provided. |
| `email` | `email` | Conditional | Required when `phone` is not sent. |
| `phone` | `string` | Conditional | Required when `email` is not sent. |
| `content` | `string` | Yes | Message (max 5000 characters). |
| `name` | `string` | No | Sender name. |
| `user_uuid` | `uuid` | No | Associates the message with an existing user. |

> The visitor IP is inferred automatically (`ip_address`). If `anonymous=true` is provided, the service uses `anonymous@pigeons.email` as the default email.

### Request example

**With reCAPTCHA (web browsers):**
```json
{
    "type": "general",
    "language": "en",
    "g_recaptcha": "recaptcha_token",
    "email": "maria.silva@example.com",
    "content": "I'd like to learn more about the plans.",
    "name": "Maria Silva"
}
```

**With X-APP-NAME header (mobile apps):**
```json
{
    "type": "general",
    "language": "en",
    "email": "maria.silva@example.com",
    "content": "I'd like to learn more about the plans.",
    "name": "Maria Silva"
}
```
> When `X-APP-NAME` header is provided, the `g_recaptcha` field is not required.

### Responses

- **201 Created**
  ```json
  {
      "data": {
          "uuid": "2c3f9d26-4c68-4478-8d1f-9a1ea3b6b32e"
      }
  }
  ```
- **422 Unprocessable Entity** - validation failure (required fields or reCAPTCHA when `X-APP-NAME` is not provided).
- **403 Forbidden** - invalid or missing `X-PUBLIC-KEY`.

---

## Attach image (`POST /api/v1/contact/{message}/images`)

Allows attaching files to the contact right after creation. The `{message}` identifier corresponds to the `uuid` returned when the message was created.

### Headers

| Header | Type | Required | Description |
|--------|------|----------|-------------|
| `X-PUBLIC-KEY` | `string` | Yes | Platform public key. |
| `Content-Type` | `multipart/form-data` | Conditional | Required when sending `image_file`; can be `application/json` when using URLs/Base64. |

### Body (main fields)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `usage` | `string` | Yes | Image purpose (e.g., `platform_contact`). |
| `name` | `string` | Conditional | Required when `image_url` is not provided. |
| `image_url` | `url` | Conditional | Public image URL (`<= 500` chars). |
| `image_encoded` | `string` | Conditional | Base64-encoded image content. |
| `image_file` | `file` | Conditional | File upload (`jpeg,png,jpg,gif,svg,webp,heic,heif` up to 2 MB). |
| `unique_id` | `string` | No | Optional identifier for deduplication. |
| `type` | `string` | No | Free-form image category. |

> You must provide **exactly one** of the fields `image_url`, `image_encoded`, or `image_file`.

### Success response (201 Created)

```json
{
    "data": {
        "uuid": "a1dcba3c-4c74-11ef-92b0-acde48001122",
        "unique_id": "5f2c19f4",
        "usage": "platform_contact",
        "url": "https://cdn.example.com/platform/contacts/a1dcba3c.png",
        "name": "receipt.png",
        "slug": "receipt-png",
        "width": 1280,
        "height": 720,
        "creation_date": "2024-06-01T13:45:00+00:00"
    }
}
```

- **404 Not Found** - when the `uuid` does not belong to the platform.
- **422 Unprocessable Entity** - invalid payload or unsupported file type.

---

## List messages (`GET /api/v1/contacts`)

Returns a paginated collection limited to the authenticated user's permissions.

### Headers

| Header | Type | Required | Description |
|--------|------|----------|-------------|
| `Authorization` | `Bearer <token>` | Yes | Valid Sanctum token. |
| `X-PUBLIC-KEY` | `string` | Yes | Current platform. |
| `Accept-Language` | `string` | No | Messages locale. |

### Query parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| `page` | `integer` | Current page (default `1`). |
| `per_page` | `integer` | Items per page (default `25`). |
| `no_paginate` | `boolean` | When `true`, returns a simple collection. |
| `limit` | `integer` | Item limit when `no_paginate=true`. |
| `sort_by` | `string` | Sorting column (default `created_at`). |
| `direction` | `string` | `ASC`/`DESC` order (default `DESC`). |
| `uuid` | `uuid` | Filters by exact identifier. |
| `email` | `email` | Contact email. |
| `phone` | `string` | Provided phone number. |
| `name` | `string` | Name (like search). |
| `type` | `string` | Type (`default`, `denunciation`, `report`). |
| `message_lang` | `string` | Message language (`pt-BR`, `en`, `es`, `gn`). |
| `created_at` | `date` | Exact date (`YYYY-MM-DD`). |
| `interval` | `string` | Quick ranges: `hour`, `day`, `week`, `month`, `year`. |
| `period.from` / `period.to` | `date` | Custom date range. |
| `user` | `string` | ID, UUID, email, or part of the author name. |
| `platform` | `string` | ID, UUID, slug, or part of the platform name. |
| `ip_address` | `ipv4` | Sender IP. |
| `ip_banned` | `boolean` | Only IPs marked as banned. |
| `is_robot` | `boolean` | Only IPs identified as bots. |
| `is_read` | `boolean` | Filters by read status. |
| `first_reader` | `string` | Identifier of the first reader. |
| `content` | `string` | Text search on message body. |
| `with_images` | `boolean` | Only messages with attachments. |

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
            "user": {
                "uuid": "1fdf0b1e-2cde-4b12-93f5-02d41a0f38c4",
                "name": "Maria Silva",
                "email": "maria.silva@example.com"
            },
            "content": "I'd like to learn more about the plans..."
        }
    ],
    "links": {
        "first": "https://api.example.com/api/v1/contacts?page=1",
        "last": "https://api.example.com/api/v1/contacts?page=5",
        "prev": null,
        "next": "https://api.example.com/api/v1/contacts?page=2"
    },
    "meta": {
        "current_page": 1,
        "from": 1,
        "last_page": 5,
        "path": "https://api.example.com/api/v1/contacts",
        "per_page": 25,
        "to": 25,
        "total": 112
    }
}
```

> The `content` field is truncated (220 characters) in the listing. Use the detail endpoint to retrieve the full body.

- **403 Forbidden** - user without permission for the platform.

---

## Show message (`GET /api/v1/contacts/{message}`)

Returns every field for the specified message.

### Success response (200 OK)

```json
{
    "data": {
        "uuid": "2c3f9d26-4c68-4478-8d1f-9a1ea3b6b32e",
        "name": "Maria Silva",
        "is_read": true,
        "email": "maria.silva@example.com",
        "created_at": "2024-06-06T18:12:45+00:00",
        "type": "default",
        "language": "pt-BR",
        "phone": "+55 11912345678",
        "content": "I'd like to learn more about the plans and available integrations...",
        "ip_address": "200.200.200.5",
        "images": [
            {
                "uuid": "a1dcba3c-4c74-11ef-92b0-acde48001122",
                "url": "https://cdn.example.com/platform/contacts/a1dcba3c.png",
                "usage": "platform_contact"
            }
        ],
        "user": {
            "uuid": "1fdf0b1e-2cde-4b12-93f5-02d41a0f38c4",
            "name": "Maria Silva",
            "email": "maria.silva@example.com"
        },
        "platform": {
            "uuid": "7a3b6d12-4d9f-4f5c-b41b-6ce39e04d57f",
            "name": "Echosistema"
        },
        "first_reader": {
            "uuid": "f39af7fa-6622-4c46-ba0d-fefb699b10f8",
            "name": "John Admin",
            "email": "john.admin@example.com"
        }
    }
}
```

- **403 Forbidden** - user without permission and not the author.
- **404 Not Found** - message does not belong to the platform.

---

## Toggle read (`PATCH /api/v1/contacts/{message}/toggle-read`)

Inverts the `is_read` state and records the first reader (`first_reader`) when applicable. Returns the same payload as the detail endpoint.

- **200 OK** - message updated.
- **403 Forbidden** - missing permission or platform mismatch.

---

## Best practices and considerations

- Always send `X-PUBLIC-KEY` or `public_key` with the correct platform value.
- After creating the message, wait for the response `uuid` before attaching images.
- Attachments are limited to 2 MB per file. Use `image_url` for larger assets already hosted elsewhere.
- Use filters (`is_read=false`, `with_images=true`, etc.) to prioritize important backoffice queues.
- Treat 403 responses as missing permissions for the current role; review the user's assigned permissions.
