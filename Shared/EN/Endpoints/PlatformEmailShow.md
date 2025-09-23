# Platform – Email Show Endpoint (IMAP)

## Endpoint

`GET /platform/app/emails/{uid}`

Returns a single IMAP message by `uid`, focused on the fields used by the UI. Headers and attachment counting are optional to preserve performance.

---

## Authentication

Required — `auth:sanctum` (Bearer token for the platform backoffice).

---

## Request

### Required Headers

| Header           | Type     | Description |
| ---------------- | -------- | ----------- |
| Authorization    | `string` | `Bearer {token}`. |
| X-PUBLIC-KEY     | `string` | Platform public key. |
| Accept-Language  | `string` | IETF language (e.g., `en`, `pt-BR`, `es`). |

### Query Parameters

| Param                        | Type      | Default | Description |
| ---------------------------- | --------- | ------- | ----------- |
| `include_headers`            | `boolean` | `false` | When `true`, includes raw headers (extra cost). |
| `include_attachment_count`   | `boolean` | `false` | When `true`, returns attachment count (may be costly depending on the server). |

---

## Response

The response follows the standard shape with `data`.

### Example (basic)

```json
{
  "data": {
    "uid": 12345,
    "sender_full": "Support Team <support@example.com>",
    "from": "support@example.com",
    "date": "2025-09-10T15:43:12+00:00",
    "headers": null,
    "body_raw": "<html>...content...</html>",
    "body": "...text content...",
    "attachments_count": null
  }
}
```

### Example (with headers and attachments)

Request: `GET /platform/app/emails/{uid}?include_headers=1&include_attachment_count=1`

```json
{
  "data": {
    "uid": 12345,
    "sender_full": "Support Team <support@example.com>",
    "from": "support@example.com",
    "date": "2025-09-10T15:43:12+00:00",
    "headers": {
      "subject": "Welcome",
      "date": "Tue, 10 Sep 2025 15:43:12 +0000",
      "message_id": "<abc@example.com>",
      "from": "Support Team <support@example.com>",
      "to": "user@customer.com",
      "cc": "",
      "bcc": "",
      "reply_to": "",
      "sender": "Support Team <support@example.com>",
      "content_type": "text/html; charset=UTF-8"
    },
    "body_raw": "<html>...content...</html>",
    "body": "...text content...",
    "attachments_count": 2
  }
}
```

---

## Fields

- `uid` `int` — IMAP message identifier.
- `sender_full` `string|null` — Sender name and email (prefers `Sender`; falls back to `From`).
- `from` `string|null` — Email address used as fallback and displayed in the UI.
- `date` `string|null` — ISO‑8601 date.
- `headers` `object|null` — Raw headers only when `include_headers=1`.
- `body_raw` `string|null` — HTML (preferred) or plain text when HTML is not available.
- `body` `string|null` — Plain text fallback.
- `attachments_count` `int|null` — Attachment count only when `include_attachment_count=1`.

---

## Notes

- IMAP flags are not fetched in show to reduce latency.
- Avoid requesting headers and attachment count when not necessary.
- Date normalization aims for ISO‑8601; some servers may return different formats.
- Localisation: text fields may reflect the `Accept-Language` header when applicable; otherwise defaults are returned.
