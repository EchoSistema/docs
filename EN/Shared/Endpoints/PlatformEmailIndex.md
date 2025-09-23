# Platform – Email Listing Endpoint (IMAP)

## Endpoint

`GET /platform/app/emails`

Returns IMAP emails from the platform-configured mailbox. Supports pagination, ordering, and field projection for better performance.

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

| Param                        | Type       | Default | Description |
| ---------------------------- | ---------- | ------- | ----------- |
| `page`                       | `integer`  | `1`     | Current page. |
| `per_page`                   | `integer`  | `10`    | Items per page (1–100). |
| `order`                      | `string`   | `desc`  | Chronological order: `asc` or `desc`. |
| `fields`                     | `string`   | —       | Comma-separated list for projection (e.g., `subject,sender_name,sender_full,date,uid,seen`). Recommended for performance. |
| `include_flags`              | `boolean`  | `true`  | Include IMAP flags when needed. Auto-inferred when any flag field is requested via `fields` (e.g., `seen`). |
| `include_body`               | `boolean`  | `false` | Include message body (expensive; request only when needed). |
| `include_body_preview`       | `boolean`  | `false` | Include 300-char plain-text preview. |
| `include_attachment_count`   | `boolean`  | `false` | Count attachments (may be costly depending on the server). |
| `include_headers`            | `boolean`  | `false` | Include raw headers (extra cost). |

---

## Response

The response follows the standard shape with `data` and `meta`.

### Example (lean projection)

Recommended for index: `?fields=subject,sender_name,sender_full,date,uid,seen`.

```json
{
  "data": [
    {
      "subject": "Welcome",
      "sender_name": "Support Team",
      "sender_full": "Support Team <support@example.com>",
      "date": "2025-09-10T15:43:12+00:00",
      "uid": 12345,
      "seen": false
    }
  ],
  "meta": {
    "page": 1,
    "per_page": 10,
    "has_more": true,
    "order": "desc"
  }
}
```

### Example (full payload)

Without `fields`, more fields may be returned (to/cc/bcc/reply_to, size, flags, etc.). Use only if necessary due to parsing cost.

---

## Common Fields

- Identity
  - `subject` `string`
  - `sender` `string|null` – sender email (when requested)
  - `sender_full` `string|null` – name + email
  - `sender_name` `string|null` – sender display name
  - `from`/`from_full`/`from_name` – From header variants (when requested)
- Metadata
  - `date` `string|null` – ISO‑8601
  - `uid` `int|null`
  - `msgno`, `message_id`, `size` (when requested)
- Flags (when `include_flags` or requested via `fields`)
  - `flags` `array|null`
  - `seen`, `answered`, `flagged`, `draft`, `recent`, `deleted` `boolean|null`
- Content (expensive; request only when needed)
  - `body_preview` `string|null`
  - `body_raw` `string|null`
  - `body` `string|null`
  - `headers` `object|null`
- Attachments (variable cost)
  - `has_attachments` `boolean|null`
  - `attachments_count` `int|null`

---

## Notes

- Use `fields` to reduce latency and IMAP workload.
- `seen` and other flags are only fetched when requested, saving IMAP calls.
- `attachments_count` can be costly; enable only when necessary.
- Dates are normalized to ISO‑8601 when possible.
- Localisation: text fields may reflect the `Accept-Language` header when applicable; otherwise defaults are returned.
