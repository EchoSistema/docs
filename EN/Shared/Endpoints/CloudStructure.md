# Shared – Cloud Storage Structure

## Endpoint

```
GET /api/v1/platform/cloud/structure?folder={folder}
```

## Description

Retrieves the folder and file structure from cloud storage (S3) for a specific folder path. Returns first-level subfolders and files along with their complete paths. The endpoint validates that the requested folder belongs to the current platform.

## Authentication

Required – Bearer {token} with appropriate ability

## Headers

| Header             | Type     | Required    | Description |
| ------------------ | -------- | ----------- | ----------- |
| Authorization      | string   | Yes         | `Bearer {token}` credential. |
| X-PUBLIC-KEY       | string   | Yes         | Platform public key. |

## Parameters

### Query parameters

| Parameter | Type   | Required | Description |
| --------- | ------ | -------- | ----------- |
| folder    | string | Yes      | Complete folder path in S3. Must contain the platform's domain and name slug. Format: `{type}/{domain-slug}/{platform-slug}` or any subfolder within. |

## Examples

### Request example (curl)

#### Root platform folder
```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  "https://api.example.com/api/v1/platform/cloud/structure?folder=files/real-estate/my-platform"
```

#### Subfolder
```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  "https://api.example.com/api/v1/platform/cloud/structure?folder=files/real-estate/my-platform/uploads"
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

### Response example - Empty folder (200)

```json
{
  "data": {
    "folders": [],
    "files": [],
    "base_path": "files/real-estate/my-platform/empty-folder",
    "path": "files/real-estate/my-platform/empty-folder"
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
| folders            | array    | List of first-level subfolders in the requested path |
| files              | array    | List of files directly in the requested path (not in subfolders) |
| base_path          | string   | The requested folder path (same as the folder parameter) |
| path               | string   | The requested folder path (same as `base_path`, maintained for compatibility) |

### folders[] Object

| Field              | Type     | Description |
| ------------------ | -------- | ----------- |
| title              | string   | Folder name (last segment of the path) |
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
- **404 Not Found**: Folder does not belong to the current platform or does not exist
- **422 Unprocessable Entity**: Validation error (missing or invalid folder parameter)
- **500 Internal Server Error**: Error accessing S3 storage

## Errors

### Validation error - Missing folder (422)

When the `folder` parameter is not provided:

```json
{
  "message": "The given data was invalid.",
  "errors": {
    "folder": [
      "The folder field is required."
    ]
  }
}
```

### Validation error - Invalid folder type (422)

When the `folder` parameter is not a string:

```json
{
  "message": "The given data was invalid.",
  "errors": {
    "folder": [
      "The folder must be a string."
    ]
  }
}
```

### Folder not found or unauthorized (404)

When the requested folder does not contain the platform's domain and name slug:

```json
{
  "message": "folder not found"
}
```

### Storage access error (500)

```json
{
  "message": "Error accessing cloud storage"
}
```

## Business Rules

### 1. Platform Validation
- The endpoint validates that the folder path contains `/{domain-slug}/{platform-slug}`
- Example: For platform "My Platform" in domain "real-estate", the path must contain `/real-estate/my-platform`
- This prevents users from accessing folders of other platforms
- Returns 404 if validation fails

### 2. Request Validation
- The `folder` parameter is required and must be a string
- Returns 422 if validation fails
- The folder parameter must be URL-encoded if it contains special characters

### 3. Path Format
- **Complete path required**: The folder parameter must be a complete S3 path
- **Valid examples**:
  - `files/real-estate/my-platform` (root platform folder)
  - `files/real-estate/my-platform/uploads` (subfolder)
  - `images/real-estate/my-platform/avatars/2024` (nested subfolder)
- **Invalid examples**:
  - `my-platform` (incomplete)
  - `files/other-domain/other-platform` (different platform - returns 404)

### 4. Folder Listing
- Returns only first-level subdirectories of the requested path
- Does not recurse into nested folders
- Folder names are extracted from file paths (no empty folders are shown if they don't contain files)

### 5. File Filtering
- Only files directly in the requested path are included
- Files in subdirectories are excluded
- `.echo` files are excluded from the listing (internal system use)

### 6. Empty Results
- If no folders exist in the path, `folders` array will be empty
- If no files exist in the path, `files` array will be empty
- Both arrays can be empty simultaneously (valid response for empty folders)

### 7. URL Encoding
- Folder paths with special characters must be URL-encoded in the query parameter
- Example: `files/real-estate/my-platform/my folder` becomes `?folder=files%2Freal-estate%2Fmy-platform%2Fmy%20folder`

## Security

### Platform Isolation
The endpoint enforces strict platform isolation:
- Users can only access folders within their authenticated platform's namespace
- The validation checks that the folder path contains the platform's domain and name slug
- Attempts to access other platforms' folders will result in 404 errors

### Path Traversal Prevention
- The validation mechanism prevents path traversal attacks
- Users cannot use `..` or other tricks to escape their platform's folder

### Request Validation
- Laravel request validation ensures the folder parameter is a string
- Prevents injection attacks through type validation

## Performance Considerations

- The endpoint lists all files in the requested path to determine structure
- Performance may vary based on the number of files in the folder
- Large folders with thousands of files may take longer to process
- Results are not cached
- Consider pagination for folders with many items (not currently implemented)

## Related Endpoints

- [CloudImages](CloudImages.md) - Get images directory statistics
- [CloudFiles](CloudFiles.md) - Get files directory statistics

## Use Cases

### 1. File Browser Navigation
Build a hierarchical file browser that allows users to navigate through their cloud storage:

```javascript
// Navigate to root
GET /api/v1/platform/cloud/structure?folder=files/real-estate/my-platform

// User clicks "uploads" folder
GET /api/v1/platform/cloud/structure?folder=files/real-estate/my-platform/uploads

// User clicks "2024" subfolder
GET /api/v1/platform/cloud/structure?folder=files/real-estate/my-platform/uploads/2024
```

### 2. Folder Selection Widget
Display available folders for file upload or organization:

```javascript
// Get available folders
GET /api/v1/platform/cloud/structure?folder=files/real-estate/my-platform

// Show folders to user: "uploads", "documents", "reports"
// User selects "documents" for file upload
```

### 3. Storage Overview Dashboard
Show folder structure and contents in a dashboard:

```javascript
// Get root structure
GET /api/v1/platform/cloud/structure?folder=files/real-estate/my-platform

// Display:
// - Folders: uploads (click to expand), documents, reports
// - Files: readme.txt, config.json
```

### 4. Breadcrumb Navigation
Build breadcrumb navigation for folder hierarchy:

```javascript
// Current path: files/real-estate/my-platform/uploads/2024/january
// Breadcrumb: Home > uploads > 2024 > january
// Each segment is clickable and calls the structure endpoint
// Home: ?folder=files/real-estate/my-platform
// uploads: ?folder=files/real-estate/my-platform/uploads
// 2024: ?folder=files/real-estate/my-platform/uploads/2024
// january: ?folder=files/real-estate/my-platform/uploads/2024/january
```

## Implementation Notes

### Platform Detection
The endpoint automatically detects the current platform from the authenticated user's context using `App::platform()`. The validation ensures the requested folder belongs to this platform.

### Request Validation
Uses Laravel's request validation (`$request->validate(['folder' => 'string'])`) to ensure the folder parameter is present and is a string.

### Folder Name Extraction
Folder names are extracted from file paths. If a folder is completely empty (no files at any depth), it won't appear in the results.

### Path Components
The path is split into components to extract first-level folders. For example:
- Request path: `files/real-estate/my-platform`
- File: `files/real-estate/my-platform/uploads/document.pdf`
- Extracted folder: `uploads`

## Integration Examples

### JavaScript/Fetch
```javascript
const folder = 'files/real-estate/my-platform';
const response = await fetch(
  `/api/v1/platform/cloud/structure?folder=${encodeURIComponent(folder)}`,
  {
    headers: {
      'Authorization': `Bearer ${token}`,
      'X-PUBLIC-KEY': publicKey
    }
  }
);
const data = await response.json();
```

### Axios
```javascript
const response = await axios.get('/api/v1/platform/cloud/structure', {
  params: { folder: 'files/real-estate/my-platform' },
  headers: {
    'Authorization': `Bearer ${token}`,
    'X-PUBLIC-KEY': publicKey
  }
});
```

### PHP/Guzzle
```php
$response = $client->get('/api/v1/platform/cloud/structure', [
    'query' => ['folder' => 'files/real-estate/my-platform'],
    'headers' => [
        'Authorization' => "Bearer {$token}",
        'X-PUBLIC-KEY' => $publicKey,
    ],
]);
```

## Changelog

- **2025-10-21**: Endpoint created with support for folder and file listing
- **2025-10-21**: Added platform validation to prevent cross-platform access
- **2025-10-21**: Changed parameter from path parameter to query parameter `folder`
- **2025-10-21**: Added request validation for folder parameter (must be string)
- **2025-10-21**: Added `base_path` field to response structure
- **2025-10-21**: Added localized error messages using `validation.attributes.folder`
