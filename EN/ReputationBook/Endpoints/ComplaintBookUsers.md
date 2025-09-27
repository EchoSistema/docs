# ReputationBook – Complaint-Book User Profile

## Fetch My Profile

### Endpoint

```
GET /api/v1/complaint-book/me
```

Returns data about the authenticated user, including stats and loaded relationships.

### Authentication

Required – `Bearer {token}` (`auth:sanctum`).

### Response

Matches the `ComplaintBookUserResource` structure, with key sections:

- Identifiers (`id`, `uuid`), name, email, gender, birth date.
- Counters `complaints_count` and `reviews_count`.
- Contacts grouped by type (`public_email`, `telephone`, `whatsapp`, ...), when available.
- Addresses (collection of `AddressResource`).
- Uploaded images (avatar, documents, etc.).

### HTTP Status Codes

- 200: Profile returned.
- 401: Invalid or missing token.

---

## Upload Image to My Profile

### Endpoint

```
POST /api/v1/complaint-book/me/image/upload
```

Allows attaching or updating images (avatar, documents, etc.) for the user.

### Authentication

Required – `Bearer {token}`.

### Headers

| Header | Type | Required | Description |
| ------ | ---- | -------- | ----------- |
| `Authorization` | string | Yes | Bearer credential. |
| `Content-Type` | string | Yes | `multipart/form-data` or `application/json`, depending on the payload. |

### Accepted Payload (`ImageStoreRequest`)

| Field | Type | Required | Description |
| ----- | ---- | -------- | ----------- |
| `usage` | string | Yes | Purpose of the image (e.g., `avatar`). |
| `name` | string | Conditional | Friendly name (required when `image_url` is missing). |
| `image_url` | string | Conditional | Public URL of the image. |
| `image_encoded` | string | Conditional | Base64-encoded content. |
| `image_file` | file | Conditional | Binary upload (`jpeg`, `png`, `webp`, `heic`, ...). Max 2 MB. |
| `unique_id` | string | No | Identifier to update an existing image. |
| `type` | string | No | Logical type (avatar, document, ...). |
| `raw` | array | No | Additional metadata. |

> Provide at least one of `image_url`, `image_encoded`, or `image_file`.

### Example Response (201)

```json
{
  "data": {
    "uuid": "b9b16c30-0d6f-4d77-91f9-56a553d9f9a1",
    "unique_id": "avatar-main",
    "usage": "avatar",
    "url": "https://cdn.echosistema.com/avatars/user-123.png",
    "name": "Profile picture",
    "slug": "profile-picture",
    "width": 512,
    "height": 512
  }
}
```

### HTTP Status Codes

- 201: Image stored.
- 400/422: Invalid payload or file.
- 401: Invalid token.

