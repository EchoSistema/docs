# Shared – Folder and File Structure (Cloud)

## Endpoint

```
GET /api/v1/platform/cloud/structure/{type}
```

## Authentication

Required – Bearer {token} with appropriate ability

## Headers

| Header             | Type     | Required    | Description |
| ------------------ | -------- | ----------- | ----------- |
| Authorization      | string   | Yes         | `Bearer {token}` credential. |
| X-PUBLIC-KEY       | string   | Yes         | Platform public key. |

## Parameters

### Path parameters

| Parameter | Type   | Required | Description |
| --------- | ------ | -------- | ----------- |
| type      | string | Yes      | Storage type. Accepted values: `images`, `files` |

## Examples

### Request example (curl)

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  "https://sandbox.your-domain.com/api/v1/platform/cloud/structure/files"
```

### Response example

```json
{
  "data": {
    "folders": [
      {
        "title": "uploads",
        "path": "files/domain-slug/platform-slug/uploads"
      },
      {
        "title": "documents",
        "path": "files/domain-slug/platform-slug/documents"
      }
    ],
    "files": [
      {
        "title": "readme.txt",
        "path": "files/domain-slug/platform-slug/readme.txt"
      },
      {
        "title": "config.json",
        "path": "files/domain-slug/platform-slug/config.json"
      }
    ],
    "path": "files/domain-slug/platform-slug"
  }
}
```

## Explained JSON Structure

| Field              | Type     | Description |
| ------------------ | -------- | ----------- |
| data               | object   | Object containing folder and file structure |
| data.folders       | array    | List of first-level folders |
| data.folders[].title | string | Folder name |
| data.folders[].path  | string | Complete folder path in S3 |
| data.files         | array    | List of direct files (not in subfolders) |
| data.files[].title | string | File name |
| data.files[].path  | string | Complete file path in S3 |
| data.base_path     | string   | Base storage path |
| data.path          | string   | Base storage path (duplicated for compatibility) |

## HTTP Status

- 200: Success
- 401: Unauthorized
- 403: Forbidden
- 500: Internal error

## Errors

Standard system errors:

```json
{
  "message": "Error accessing storage"
}
```

## Notes

- The `type` parameter defines the storage type to be listed
- Accepted values for `type`: `images` (images) or `files` (files)
- The endpoint returns only first-level folders and files
- `.echo` files are excluded from listing (internal system use)
- Base path is automatically built based on current platform
- Path format: `{type}/{domain-slug}/{platform-slug}`
- Does not return subfolder contents recursively

## Related

- [CloudImages](CloudImages.md) - Image statistics
- [CloudFiles](CloudFiles.md) - File statistics

## Changelog

- 2025-10-21: Endpoint created with support for folder and file listing
