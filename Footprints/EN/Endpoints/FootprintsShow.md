# Footprints – Footprint Details

## Endpoint

`GET /api/v1/footprints/{footprint}`

Retrieves detailed information about a specific footprint.

---

## Authentication

The endpoint requires authentication.

### Required Headers

| Header | Type | Description |
| --- | --- | --- |
| **Authorization** | `string` | Bearer token used for authentication. |
| **X-PUBLIC-KEY** | `string` | Public key of the platform. |
| **Accept-Language** | `string` | Language for error messages (optional). |

---

## Request

### Path Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `footprint` | `integer` | Yes | Footprint identifier. |

### Query Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `is_index` | `boolean` | No | If `true`, returns the lightweight index format. Default `false`. |

---

## Responses

### Success `200 OK`

```json
{
  "data": {
    "id": 1,
    "session_id": "11111111-2222-3333-4444-555555555555",
    "event": "page_view",
    "event_time": "2024-01-01T12:00:00-03:00",
    "page": "https://example.com/messages",
    "title": "Messages Page",
    "timezone_offset": 180,
    "language": "en",
    "connection_type": null,
    "created_at": "2024-01-01T09:00:00-03:00",
    "ip_address": "203.0.113.42",
    "is_banned": 0,
    "is_robot": 0,
    "platform": {
      "uuid": "aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee",
      "name": "Example Platform",
      "public_key": "example-public-key",
      "domain_area": {
        "uuid": "11111111-2222-3333-4444-555555555555",
        "name": "Example Domain",
        "slug": "example-domain",
        "is_active": 1
      },
      "regional_information": {
        "currency": 1,
        "language": "en-US"
      }
    },
    "user": {
      "uuid": "ffffffff-1111-2222-3333-444444444444",
      "name": "Jane Doe",
      "age": 30,
      "regional_information": {
        "currency": 1,
        "language": "en-US"
      }
    },
    "os_name": "Linux",
    "os_version": null,
    "browser_name": "Firefox",
    "browser_version": "128.0",
    "device_type": "desktop",
    "device_brand": "Generic",
    "device_model": null,
    "screen": null,
    "viewport": null
  }
}
```

### Success (Index Format) `200 OK`

Returned when `is_index=true`.

```json
{
  "data": {
    "id": 1,
    "event_time": "2024-01-01T12:00:00-03:00",
    "event": "page_view",
    "page": "https://example.com/messages",
    "title": "Messages Page",
    "session_id": "11111111-2222-3333-4444-555555555555",
    "ip_address": "203.0.113.42",
    "is_banned": 0,
    "is_robot": 0,
    "platform": {
      "uuid": "aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee",
      "name": "Example Platform"
    },
    "user": {
      "uuid": "ffffffff-1111-2222-3333-444444444444",
      "name": "Jane Doe"
    },
    "created_at": "2024-01-01T09:00:00-03:00"
  }
}
```

### Error `404 Not Found`

Returned when the footprint does not exist.

### Error `401 Unauthorized`

Returned when authentication fails.

