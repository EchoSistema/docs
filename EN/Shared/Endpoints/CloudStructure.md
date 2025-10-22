# Shared – Cloud Storage Structure

## Endpoint

```
GET /api/v1/platform/cloud/structure/{type}
```

## Description

Retrieves the folder and file structure from cloud storage (S3) for the current platform. Returns first-level folders and files along with their complete paths.

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
| type      | string | Yes      | Storage type to list. Accepted values: `images`, `files` |

## Examples

### Request example (curl)

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  "https://api.example.com/api/v1/platform/cloud/structure/files"
```

### Response example - Success (200)

```json
{
  "data": {
    "folders": [
      {
        "title": "uploads",
        "path": "files/real-estate/my-platform/uploads"
      },
      {
        "title": "documents",
        "path": "files/real-estate/my-platform/documents"
      },
      {
        "title": "reports",
        "path": "files/real-estate/my-platform/reports"
      }
    ],
    "files": [
      {
        "title": "readme.txt",
        "path": "files/real-estate/my-platform/readme.txt"
      },
      {
        "title": "config.json",
        "path": "files/real-estate/my-platform/config.json"
      }
    ],
    "base_path": "files/real-estate/my-platform",
    "path": "files/real-estate/my-platform"
  }
}
```

## Explained JSON Structure

### Response Object

| Field              | Type     | Description |
| ------------------ | -------- | ----------- |
| data               | object   | Container object for the storage structure |

### data Object

| Field              | Type     | Description |
| ------------------ | -------- | ----------- |
| folders            | array    | List of first-level folders in the storage path |
| files              | array    | List of files directly in the base path (not in subfolders) |
| base_path          | string   | Base storage path for the current platform |
| path               | string   | Base storage path (same as `base_path`, maintained for compatibility) |

### folders[] Object

| Field              | Type     | Description |
| ------------------ | -------- | ----------- |
| title              | string   | Folder name |
| path               | string   | Complete folder path in S3 storage |

### files[] Object

| Field              | Type     | Description |
| ------------------ | -------- | ----------- |
| title              | string   | File name with extension |
| path               | string   | Complete file path in S3 storage |

## HTTP Status

- **200 OK**: Structure retrieved successfully
- **401 Unauthorized**: Missing or invalid authentication token
- **403 Forbidden**: Insufficient permissions to access cloud storage
- **422 Unprocessable Entity**: Invalid `type` parameter
- **500 Internal Server Error**: Error accessing S3 storage

## Errors

### Invalid type parameter (422)

```json
{
  "message": "The given data was invalid.",
  "errors": {
    "type": [
      "The type field must be one of: images, files."
    ]
  }
}
```

### Storage access error (500)

```json
{
  "message": "Error accessing cloud storage"
}
```

## Business Rules

1. **Type Parameter**:
   - Only accepts `images` or `files` as valid values
   - Determines the storage directory to list

2. **Path Construction**:
   - Format: `{type}/{domain-slug}/{platform-slug}`
   - Example: `files/real-estate/my-platform`
   - Automatically built based on current authenticated platform

3. **Folder Listing**:
   - Returns only first-level subdirectories
   - Does not recurse into nested folders
   - Folder names are extracted from file paths

4. **File Filtering**:
   - Only files directly in the base path are included
   - Files in subdirectories are excluded
   - `.echo` files are excluded (internal system use)

5. **Empty Results**:
   - If no folders exist, `folders` array will be empty
   - If no files exist, `files` array will be empty
   - Both can be empty simultaneously

## Performance Considerations

- The endpoint lists all files in the storage path to determine structure
- Performance may vary based on the number of files in storage
- Results are not cached

## Related Endpoints

- [CloudImages](CloudImages.md) - Get images directory statistics
- [CloudFiles](CloudFiles.md) - Get files directory statistics

## Use Cases

1. **File Browser**: Build a file browser interface for the platform
2. **Navigation**: Allow users to navigate through folder structure
3. **Folder Selection**: Display available folders for file organization
4. **Storage Overview**: Provide a high-level view of stored content

## Changelog

- **2025-10-21**: Endpoint created with support for folder and file listing
- **2025-10-21**: Added `type` parameter for dynamic storage type selection
- **2025-10-21**: Added `base_path` field to response structure
