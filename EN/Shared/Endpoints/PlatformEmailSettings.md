# Platform — Email Settings Endpoint (IMAP)

## Endpoints

- `GET /platform/app/emails/settings`
- `POST /platform/app/emails/settings`
- `PUT /platform/app/emails/settings`

Manages the platform IMAP settings (host, port, encryption, certificate validation, credentials, default folder).

---

## Authentication

Required — `auth:sanctum` (Bearer token for the platform backoffice).

---

## GET /platform/app/emails/settings

Returns the current platform IMAP settings.

- Responses
  - `200 OK` with settings object (without the password)
  - `204 No Content` when no settings exist

### Response Example
```json
{
  "host": "imap.example.com",
  "port": 993,
  "encryption": "ssl",
  "validate_cert": true,
  "username": "inbox@example.com",
  "default_folder": "INBOX",
  "timeout": 30
}
```

---

## POST /platform/app/emails/settings

Creates the IMAP settings for the platform (if none exist).

- Rules
  - Fails with `422 Unprocessable Entity` if settings already exist
  - Required fields per validation

### Body (JSON)
| Field            | Type         | Req. | Description |
| ---------------- | ------------ | ---- | ----------- |
| `host`           | `string`     | Yes  | IMAP host. |
| `port`           | `integer`    | Yes  | Port (1–65535). |
| `encryption`     | `string|null`| No   | `ssl`, `tls` or `null`. |
| `validate_cert`  | `boolean`    | Yes  | Validate TLS certificate. |
| `username`       | `string`     | Yes  | IMAP username. |
| `password`       | `string`     | Yes  | IMAP password. |
| `default_folder` | `string`     | Yes  | Default folder (e.g., `INBOX`). |
| `timeout`        | `integer`    | No   | Timeout in seconds (1–600). Default: 30. |

### Request Example
```json
{
  "host": "imap.example.com",
  "port": 993,
  "encryption": "ssl",
  "validate_cert": true,
  "username": "inbox@example.com",
  "password": "secret",
  "default_folder": "INBOX",
  "timeout": 45
}
```

### Responses
- `201 Created` with success message
- `422 Unprocessable Entity` if already exists

---

## PUT /platform/app/emails/settings

Updates (or creates if missing) the platform IMAP settings.

### Body (JSON)
Same fields as POST; `password` is optional (only updated if provided).

### Responses
- `200 OK` with success message
- `422 Unprocessable Entity` on validation errors

---

## Notes

- Password is never returned for security.
- Default `timeout` is 30s when omitted.
- For performance and security, prefer `INBOX` or specific folders; avoid scanning large archives unless necessary.
- Allowed `encryption` values: `ssl`, `tls`, or `null` (no encryption — not recommended).
