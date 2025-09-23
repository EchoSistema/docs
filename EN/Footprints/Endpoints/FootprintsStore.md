# Footprints – Create Footprint

## Endpoint

`POST /api/v1/footprints`

Registers a navigation trace or visitor interaction.

---

## Authentication

None. Still, the `X-PUBLIC-KEY` header must be sent.

---

## Request

### Required Headers

| Header | Type | Description |
| ------- | ---- | ----------- |
| **X-PUBLIC-KEY** | `string` | Public key of the platform required to authenticate and receive responses. |
| **Accept-Language** | `string` | Language for error messages (e.g., `en`). Optional. |

### Body

| Field | Type | Required | Description |
| ----- | ---- | -------- | ----------- |
| `session_id` | `string` | Yes | Session identifier; uses the `x-correlation-id` header if missing. |
| `event` | `FootprintEventEnum` | Yes | Event type. |
| `event_time` | `date` | Yes | Timestamp when the event occurred. |
| `page` | `string` | Yes | Visited page URL. |
| `visitor_ip` | `ip` | No | Visitor IP; detected automatically. |
| `echo_id` | `string` | No | Mirror identifier. |
| `user` | `string` | No | Reference to the user. |
| `referrer` | `string` | No | Referrer URL. |
| `title` | `string` | No | Page title. |
| `utm.source` | `string` | No | Campaign source. |
| `utm.medium` | `string` | No | Campaign medium. |
| `utm.campaign` | `string` | No | Campaign name. |
| `utm.term` | `string` | No | Campaign term. |
| `utm.content` | `string` | No | Campaign content. |
| `utms` | `array` | No | Additional UTM data. |
| `clids` | `array` | No | Click identifiers. |
| `browser.name` | `string` | No | Browser name. |
| `browser.version` | `string` | No | Browser version. |
| `os.name` | `string` | No | Operating system. |
| `os.version` | `string` | No | OS version. |
| `device.type` | `string` | No | Device type. |
| `device.brand` | `string` | No | Device brand. |
| `device.model` | `string` | No | Device model. |
| `screen.w` | `integer` | No | Screen width. |
| `screen.h` | `integer` | No | Screen height. |
| `screen.dpr` | `numeric` | No | Screen pixel ratio. |
| `viewport.w` | `integer` | No | Viewport width. |
| `viewport.h` | `integer` | No | Viewport height. |
| `timezone_offset` | `integer` | No | Timezone offset in minutes. |
| `connection_type` | `string` | No | Connection type. |
| `language` | `string` | No | Browser language. |

### Body Example

```json
{
  "echo_id": null,
  "session_id": "00000000-0000-0000-0000-000000000000",
  "event": "page_view",
  "event_time": "2025-01-01T00:00:00.000Z",
  "page": "https://example.com/laws",
  "referrer": null,
  "title": "Sample Page Title",
  "utm": {
    "source": null,
    "medium": null,
    "campaign": null,
    "term": null,
    "content": null
  },
  "browser": {
    "name": "Edge",
    "version": "140.0.0.0"
  },
  "os": {
    "name": "Linux",
    "version": null
  },
  "device": {
    "type": "desktop",
    "brand": null,
    "model": null
  },
  "screen": {
    "w": 1920,
    "h": 1080,
    "dpr": 1
  },
  "viewport": {
    "w": 1912,
    "h": 964
  },
  "timezone_offset": 180,
  "connection_type": "4g",
  "language": "en-US",
  "platform_id": 1,
  "platform_uuid": "00000000-0000-0000-0000-000000000000",
  "platform_language": "en-US",
  "platform_currency": "USD",
  "visitor_ip": "203.0.113.1"
}
```

---

## Responses

### Success `201 Created`

```json
["success"]
```

### Error `400 Bad Request`

Returns an object with the error message.

---

## Notes

* Localisation: error messages may follow `Accept-Language` when provided.
