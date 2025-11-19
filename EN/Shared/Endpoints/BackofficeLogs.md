# Backoffice – Application Logs

## Endpoints

```
GET /api/v1/backoffice/logs              # list available log files
GET /api/v1/backoffice/logs/{file}       # retrieve content (tail) of a specific log
```

## Authentication & Permissions

- Requires Sanctum token with `backoffice` ability.
- Gate `viewLogs`: only users with `index.all` permission (e.g., *master* role) can access.

## Headers

| Header        | Type   | Required | Description |
| ------------- | ------ | -------- | ----------- |
| Authorization | string | Yes      | `Bearer {token}` with `backoffice` ability. |
| Accept        | string | No       | `application/json`. |

## Parameters

### GET /api/v1/backoffice/logs
No parameters.

### GET /api/v1/backoffice/logs/{file}
| Parameter | Type   | Required | Description |
| --------- | ------ | -------- | ----------- |
| file      | string | Yes      | Log prefix without `.log` extension. E.g., `order`, `authentication`, `logout`, `log` (maps to `laravel.log`). |
| lines     | int    | No       | Maximum number of lines returned after combining all matching files (default 200, maximum 500). |

## Request Examples

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  https://sandbox.echosistema.app/api/v1/backoffice/logs

curl -X GET \
  -H "Authorization: Bearer <token>" \
  "https://sandbox.echosistema.app/api/v1/backoffice/logs/laravel.log?lines=100"
```

## Response Examples

### Log list
```json
{
  "data": [
    {
      "file": "laravel.log",
      "size": 1048576,
      "size_human": "1.00 MB",
      "last_modified": "2025-11-19T18:00:00+00:00"
    }
  ]
}
```

### Log content
```json
{
  "data": {
    "query": "order",
    "files": [
      {
        "file": "order.log",
        "size": 34567,
        "size_human": "33.8 KB",
        "last_modified": "2025-11-19T18:00:00+00:00"
      },
      {
        "file": "order-2025-11-18.log",
        "size": 12345,
        "size_human": "12.1 KB",
        "last_modified": "2025-11-18T23:00:00+00:00"
      }
    ],
    "lines": [
      {
        "file": "order.log",
        "timestamp": "2025-11-19 18:00:01",
        "environment": "testing",
        "type": "INFO",
        "name": "Ewerton Daniel",
        "message": "Ewerton Daniel successfully authenticated.",
        "context": { "model": "Domain\\Shared\\Models\\User", "id": 24 }
      },
      {
        "file": "order-2025-11-18.log",
        "timestamp": "2025-11-18 23:59:59",
        "environment": "testing",
        "type": "ERROR",
        "message": "Failed",
        "context": { "details": "..." }
      }
    ]
  }
}
```

## HTTP Status

- 200: OK
- 401: Invalid or missing token
- 403: Missing `index.all` permission
- 404: File not found

## Notes

- Only files in `storage/logs` are exposed.
- Provide only the file prefix (without `.log`). The endpoint searches for all files starting with that prefix (e.g., `order`, `order-2025-11-17.log`) and merges the lines.
- Each line is converted to an object with `timestamp`, `environment`, `type`, `name` (excerpt between `:` and `successfully`), `message` and `context` (when the log is in standard Laravel format). If the line doesn't match the pattern, the `line` field is returned with the raw content.
- The returned content is a consolidated *tail*; use `lines` to define the maximum number of aggregated lines.
- The value `log` is automatically converted to `laravel.log` for backward compatibility.

## Changelog

- 2025-11-19: Endpoint created for reading logs in backoffice.
