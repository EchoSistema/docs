# Shared – Upload Contact Message Image

## Endpoint

`POST /api/v1/contact/{message}/images`

## Authentication

None (public endpoint). Requires platform header.

## Headers

| Header | Type | Required | Description |
| ------ | ---- | -------- | ----------- |
| X-PUBLIC-KEY | `string` | Yes | Platform public key. |
| Content-Type | `multipart/form-data` | Conditional | Required when sending `image_file`; can be `application/json` when using URLs/Base64. |

## Parameters

Upload an image attachment to a contact message.

Path parameter:
- `message` (uuid) — message identifier returned by create endpoint.

### Body

| Field | Type | Required | Description |
| ----- | ---- | -------- | ----------- |
| `usage` | `string` | Yes | Image purpose (e.g., `platform_contact`). |
| `name` | `string` | Required without `image_url` | Original filename. |
| `image_url` | `url` | Required without `image_file`/`image_encoded` | Public URL (max 500 chars). |
| `image_encoded` | `string` | Required without `image_file`/`image_url` | Base64-encoded image. |
| `image_file` | `file` | Required without `image_url`/`image_encoded` | `jpeg,png,jpg,gif,svg,webp,heic,heif` up to 2 MB. |
| `unique_id` | `string` | No | Optional deduplication key. |
| `type` | `string` | No | Free-form category.

## Examples

### Request example (curl)

```bash
curl -X POST \
  
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: en" \
  "https://sandbox.your-domain.com/api/v1/contact/{message}/images"
```

### Success `200 OK`

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

## JSON Structure Explained

| Field | Type | Description |
| ----- | ---- | ----------- |
| data  | `object`  | Image resource data. |

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

- Upload an image attachment to a contact message

## Related

- See other Shared API endpoints

## Changelog

- 2025-10-16: Initial documentation
