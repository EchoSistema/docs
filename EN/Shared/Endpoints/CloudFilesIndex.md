# Shared – File Storage Statistics (S3)

## Endpoint

```
GET /api/v1/cloud/files
```

## Authentication

Required – Bearer {token}

## Headers

| Header           | Type   | Required | Description |
| ---------------- | ------ | -------- | ----------- |
| Authorization    | string | Yes | `Bearer {token}`. |
| X-PUBLIC-KEY     | string | Yes      | Platform public key. |
| Accept-Language  | string | No       | IETF locale (e.g., `pt-BR`, `en`, `es`). |

## Parameters

No additional parameters required. The endpoint returns statistics for the current platform's file directory on AWS S3.

## Examples

### Request example (curl)

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: en" \
  "https://sandbox.your-domain.com/api/v1/cloud/files"
```

### Response example

```json
{
  "data": {
    "path": "files/domain-slug/platform-name",
    "files": 28,
    "directories": 5,
    "total_size": 5242880,
    "total_size_formatted": "5 MB",
    "timestamp": "2025-10-21 10:30:45"
  }
}
```

## JSON Structure Explained

| Field | Type | Description |
| ----------- | ------- | ----------- |
| data        | object  | File directory statistics |
| data.path   | string  | Relative path in S3 bucket (format: `files/{domain-slug}/{platform-name-slug}`) |
| data.files  | integer | Total number of files (excluding .echo) |
| data.directories | integer | Total number of subdirectories |
| data.total_size | integer | Total size in bytes |
| data.total_size_formatted | string | Formatted size in human-readable format (KB, MB, GB, etc) |
| data.timestamp | string | Query date and time (format: Y-m-d H:i:s) |

## HTTP Status

- 200: OK
- 401: Unauthorized
- 403: Forbidden
- 500: Internal Server Error (possible S3 connection failure)

## Errors

```json
{
  "message": "Error message"
}
```

Possible specific errors:
- Invalid or expired S3 credentials
- Insufficient permissions on S3 bucket
- AWS S3 connection timeout

## Notes

- The endpoint automatically creates the `.echo` file in S3 if it doesn't exist
- Each query appends a new line to the `.echo` file with timestamp and statistics
- The `.echo` file is excluded from file count and size calculation
- The folder structure in S3 is based on the domain slug and current platform name
- Path format: `files/{domain-slug}/{platform-name-slug}/`
- Calculation is done recursively including all subdirectories

## Related

- [Image Statistics](CloudImagesIndex.md)

## Changelog

- 2025-10-21: Initial documentation
