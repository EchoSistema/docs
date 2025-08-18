# Footprints – Footprint Details

## Endpoint

`GET /footprints/{footprint}`

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
    "id": 813,
    "session_id": "8e007f7f-b9bb-4376-b675-8ade018dfdd3",
    "event": "page_view",
    "event_time": "2025-08-17T00:44:24-03:00",
    "page": "http://localhost:5173/my-messages",
    "title": "Libro de Quejas - Mensajes",
    "timezone_offset": 180,
    "language": "es",
    "connection_type": null,
    "created_at": "2025-08-16T21:44:24-03:00",
    "ip_address": "181.120.47.200",
    "is_banned": 0,
    "is_robot": 0,
    "platform": {
      "uuid": "9bcf5ae4-f7e6-3276-9755-3b3ef52ef03d",
      "name": "Defensa del Consumidor - Encarnación",
      "public_key": "c43864ce-efae-31c9-bb66-82dceefbc996",
      "domain_area": {
        "uuid": "9a87934f-4adb-56a5-8c3f-34efc1b91881",
        "name": "ReputationBook",
        "slug": "reputationbook",
        "is_active": 1
      },
      "regional_information": {
        "currency": 1,
        "language": "pt-BR"
      }
    },
    "user": {
      "uuid": "c1f1f474-4346-3118-83c5-2b5a6e6aff48",
      "name": "Ewerton Girardi",
      "age": 39,
      "regional_information": {
        "currency": 1,
        "language": "pt-BR"
      }
    },
    "os_name": "Linux",
    "os_version": null,
    "browser_name": "Firefox",
    "browser_version": "128.0",
    "device_type": "desktop",
    "device_brand": "0",
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
    "id": 813,
    "event_time": "2025-08-17T00:44:24-03:00",
    "event": "page_view",
    "page": "http://localhost:5173/my-messages",
    "title": "Libro de Quejas - Mensajes",
    "session_id": "8e007f7f-b9bb-4376-b675-8ade018dfdd3",
    "ip_address": "181.120.47.200",
    "is_banned": 0,
    "is_robot": 0,
    "platform": {
      "uuid": "9bcf5ae4-f7e6-3276-9755-3b3ef52ef03d",
      "name": "Defensa del Consumidor - Encarnación"
    },
    "user": {
      "uuid": "c1f1f474-4346-3118-83c5-2b5a6e6aff48",
      "name": "Ewerton Girardi"
    },
    "created_at": "2025-08-16T21:44:24-03:00"
  }
}
```

### Error `404 Not Found`

Returned when the footprint does not exist.

### Error `401 Unauthorized`

Returned when authentication fails.

