# Shared – Admin Update User Image

## Endpoint

`POST /api/v1/users/{uuid}/image`

## Authentication

Requires a valid Bearer token with `backoffice` ability and appropriate permissions (`update.guest`, `update.collaborator`, or `update.all`).

## Headers

| Header | Type | Required | Description |
| ------ | ---- | -------- | ----------- |
| Authorization | `string` | Yes | `Bearer <token>` with backoffice ability. |
| X-PUBLIC-KEY  | `string` | Yes | Platform public key. |
| Accept-Language | `string` | No | Locale for validation messages. |
| Content-Type | `string` | Yes | `multipart/form-data` for file upload or `application/json` for URL/encoded. |

## URL Parameters

| Parameter | Type | Required | Description |
| --------- | ---- | -------- | ----------- |
| `uuid` | `uuid` | Yes | UUID of the user whose avatar will be updated. |

## Body

The request supports three different methods to provide the image:

### Method 1: File Upload (multipart/form-data)

| Field | Type | Required | Description |
| ----- | ---- | -------- | ----------- |
| `name` | `string` | Yes | Image name (max 255). |
| `usage` | `string` | Yes | Image usage type (e.g., "avatar"). |
| `image_file` | `file` | Yes | Image file (jpeg, png, jpg, gif, svg, webp, heic, heif). Max 2MB. |
| `type` | `string` | No | Image type identifier (max 255). |
| `raw` | `array` | No | Additional metadata. |

### Method 2: Image URL (application/json)

| Field | Type | Required | Description |
| ----- | ---- | -------- | ----------- |
| `name` | `string` | Yes | Image name (max 255). |
| `usage` | `string` | Yes | Image usage type (e.g., "avatar"). |
| `image_url` | `url` | Yes | URL to the image (max 500). |
| `type` | `string` | No | Image type identifier (max 255). |
| `raw` | `array` | No | Additional metadata. |

### Method 3: Base64 Encoded (application/json)

| Field | Type | Required | Description |
| ----- | ---- | -------- | ----------- |
| `name` | `string` | Yes | Image name (max 255). |
| `usage` | `string` | Yes | Image usage type (e.g., "avatar"). |
| `image_encoded` | `string` | Yes | Base64 encoded image data. |
| `type` | `string` | No | Image type identifier (max 255). |
| `raw` | `array` | No | Additional metadata. |

## Example Request (File Upload)

```bash
curl -X POST https://api.example.com/api/v1/users/550e8400-e29b-41d4-a716-446655440000/image \
  -H "Authorization: Bearer <admin-token>" \
  -H "X-PUBLIC-KEY: <platform-key>" \
  -F "name=User Avatar" \
  -F "usage=avatar" \
  -F "image_file=@/path/to/avatar.jpg"
```

## Example Request (JSON with URL)

```json
{
  "name": "User Avatar",
  "usage": "avatar",
  "image_url": "https://example.com/images/user-avatar.jpg"
}
```

## Success `200 OK`

```json
{
  "data": {
    "uuid": "550e8400-e29b-41d4-a716-446655440000",
    "unique_id": "abc123def456",
    "usage": "avatar",
    "url": "https://cdn.example.com/users/john/avatar-abc123def456.webp",
    "name": "User Avatar",
    "slug": "user-avatar",
    "width": 512,
    "height": 512,
    "creation_date": "2024-01-15T10:30:00+00:00"
  }
}
```

## JSON Structure

| Field | Type | Description |
| ----- | ---- | ----------- |
| `data.uuid` | `uuid` | Image identifier. |
| `data.unique_id` | `string` | Hexadecimal unique identifier. |
| `data.usage` | `string` | Image usage type. |
| `data.url` | `string` | Full URL to access the image. |
| `data.name` | `string` | Image name. |
| `data.slug` | `string` | URL-friendly version of the name. |
| `data.width` | `integer|null` | Image width in pixels. |
| `data.height` | `integer|null` | Image height in pixels. |
| `data.creation_date` | `datetime` | When the image was created. |

## Errors

- 401 Unauthorized — missing/invalid token or insufficient permissions
- 403 Forbidden — user does not have required permissions to update this user
- 404 Not Found — user with specified UUID not found
- 422 Unprocessable Entity — validation errors
- 500 Internal Server Error

### 403 Example

```json
{
  "message": "You do not have permission to perform this action."
}
```

### 422 Example

```json
{
  "message": "The given data was invalid.",
  "errors": {
    "name": ["The name field is required when image url is not present."],
    "usage": ["The usage field is required."],
    "image_file": ["The image file must be a file of type: jpeg, png, jpg, gif, svg, webp, heic, heif."]
  }
}
```

## Permissions

The authenticated user must have one of the following permissions:
- `update.guest` — Can update guest users
- `update.collaborator` — Can update collaborator users
- `update.all` — Can update all users

## Notes

- You must provide **exactly one** of: `image_file`, `image_url`, or `image_encoded`.
- The `usage` field should be set to `"avatar"` for user avatars.
- Uploaded images are automatically optimized and converted to WebP format.
- The previous avatar image will be replaced with the new one.
- This endpoint requires `backoffice` token ability.

## Related

- [Get User Profile](./UserProfile.md)
- [Admin Update User](./AdminUserUpdate.md)
- [Update My Image](./UserImageUpdate.md)
