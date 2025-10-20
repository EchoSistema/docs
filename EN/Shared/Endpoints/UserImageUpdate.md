# Shared – Update My Image

## Endpoint

`POST /api/v1/me/image`

## Authentication

Requires a valid Bearer token.

## Headers

| Header | Type | Required | Description |
| ------ | ---- | -------- | ----------- |
| Authorization | `string` | Yes | `Bearer <token>`. |
| X-PUBLIC-KEY  | `string` | Yes | Platform public key. |
| Accept-Language | `string` | No | Locale for validation messages. |
| Content-Type | `string` | Yes | `multipart/form-data` for file upload or `application/json` for URL/encoded. |

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
curl -X POST https://api.example.com/api/v1/me/image \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <platform-key>" \
  -F "name=My Avatar" \
  -F "usage=avatar" \
  -F "image_file=@/path/to/avatar.jpg"
```

## Example Request (JSON with URL)

```json
{
  "name": "My Avatar",
  "usage": "avatar",
  "image_url": "https://example.com/images/avatar.jpg"
}
```

## Example Request (JSON with Base64)

```json
{
  "name": "My Avatar",
  "usage": "avatar",
  "image_encoded": "data:image/jpeg;base64,/9j/4AAQSkZJRgABAQEAYABgAAD..."
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
    "name": "My Avatar",
    "slug": "my-avatar",
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

- 401 Unauthorized — missing/invalid token
- 422 Unprocessable Entity — validation errors
- 500 Internal Server Error

### 422 Example

```json
{
  "message": "The given data was invalid.",
  "errors": {
    "name": ["The name field is required when image url is not present."],
    "usage": ["The usage field is required."],
    "image_file": ["The image file must be a file of type: jpeg, png, jpg, gif, svg, webp, heic, heif."],
    "image_url": ["The image url must be a valid URL."]
  }
}
```

## Notes

- You must provide **exactly one** of: `image_file`, `image_url`, or `image_encoded`.
- The `usage` field should be set to `"avatar"` for user avatars.
- Uploaded images are automatically optimized and converted to WebP format.
- The previous avatar image will be replaced with the new one.

## Related

- [Get User Profile](./UserProfile.md)
- [Update User Profile](./UserProfileUpdate.md)
- [Admin Update User Image](./AdminUserImageUpdate.md)
