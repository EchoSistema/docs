# Footprints – Index Endpoint

## Endpoint

`GET /api/v1/footprints`

Retrieves a list of footprints with optional filtering and pagination.

---

## Authentication

Requires a valid Sanctum token.

### Required Headers

| Header | Type | Description |
| ------ | ---- | ----------- |
| **Authorization** | `string` | `Bearer <token>` required. |
| **X-PUBLIC-KEY** | `string` | Public key identifying the platform. Required. |
| **Accept-Language** | `string` | Locale for translatable fields. Optional. |

---

## Request

### Filter Parameters

#### Footprint IP

| Parameter | Type | Description |
| --------- | ---- | ----------- |
| `ip_address` | `ip` | Visitor IP address. |
| `is_banned` | `boolean` | Filter banned IPs. |
| `is_robot` | `boolean` | Filter robot flags. |

#### Platform

| Parameter | Type | Description |
| --------- | ---- | ----------- |
| `platform_identifier` | `integer` | Platform identifier. |
| `platform_name` | `string` | Platform name. |
| `platform_email` | `email` | Platform contact email. |
| `platform_open` | `boolean` | Whether the platform is open. |
| `platform_is_parent` | `boolean` | Filter parent platforms. |
| `domain_area_id` | `integer` | Domain area ID. |
| `platform_currency_id` | `integer` | Currency ID. |
| `platform_language` | `string` | Platform language code. |
| `platform_has_company` | `boolean` | Filter by company presence. |
| `platform_country` | `string` | Country name. |
| `platform_region` | `string` | State or region name. |
| `platform_city` | `string` | City name. |
| `platform_user` | `string` | Platform user name. |

#### User

| Parameter | Type | Description |
| --------- | ---- | ----------- |
| `has_user` | `boolean` | When `true`, returns only footprints with a related user; when `false`, returns only those without a user. |
| `user_id` | `integer` | User ID. |
| `user_uuid` | `uuid` | User UUID. |
| `user_email` | `email` | User email. |
| `user_name` | `string` | User name. |
| `user_gender` | `string` | Gender. |
| `user_birth_date` | `date` | Exact birth date. |
| `user_birth_from` | `date` | Birth date start range. |
| `user_birth_to` | `date` | Birth date end range. |
| `user_email_verified` | `boolean` | Filter by email verification. |

#### Footprint

| Parameter | Type | Description |
| --------- | ---- | ----------- |
| `footprint_ip_id` | `integer` | Footprint IP ID. |
| `session_id` | `string` | Session identifier. |
| `event` | `FootprintEventEnum` | Specific event. |
| `events[]` | `array` | List of events. |
| `event_time` | `date` | Exact event time. |
| `event_from` | `date` | Event time start. |
| `event_to` | `date` | Event time end. |
| `interval` | `string` | Aggregation interval (`hour`, `day`, `week`, `month`, `year`). |
| `period[from]` | `date` | Period start. |
| `period[to]` | `date` | Period end. |
| `page_url` | `string` | Page URL. |
| `referrer` | `string` | Referring URL. |
| `title` | `string` | Page title. |
| `timezone_offset` | `integer` | Client timezone offset. |
| `connection_type` | `string` | Connection type. |
| `language` | `string` | Preferred language. |
| `is_hidden` | `boolean` | Filter hidden footprints. |

#### Query Params (UTM)

| Parameter | Type | Description |
| --------- | ---- | ----------- |
| `utm_source` | `string` | Campaign source. |
| `utm_medium` | `string` | Campaign medium. |
| `utm_campaign` | `string` | Campaign name. |
| `utm_term` | `string` | Campaign term. |
| `utm_content` | `string` | Campaign content. |
| `utms` | `string` | Other UTM values. |
| `clids` | `string` | Campaign identifiers. |

#### Device

| Parameter | Type | Description |
| --------- | ---- | ----------- |
| `device_os_name` | `string` | OS name. |
| `device_os_version` | `string` | OS version. |
| `device_browser_name` | `string` | Browser name. |
| `device_browser_version` | `string` | Browser version. |
| `device_type` | `string` | Device type. |
| `device_brand` | `string` | Device brand. |
| `device_model` | `string` | Device model. |
| `screen_w` | `integer` | Screen width. |
| `screen_h` | `integer` | Screen height. |
| `screen_dpr` | `numeric` | Device pixel ratio. |
| `viewport_w` | `integer` | Viewport width. |
| `viewport_h` | `integer` | Viewport height. |

#### Pagination & Sorting

| Parameter | Type | Description |
| --------- | ---- | ----------- |
| `page` | `integer` | Page number (default 1). |
| `per_page` | `integer` | Items per page (1-200, default 25). |
| `sort` | `string` | Sort column (`id`, `event_time`, etc.). |
| `direction` | `string` | Sort direction (`ASC` or `DESC`). |
| `no_paginate` | `boolean` | Disable pagination. |
| `limit` | `integer` | Max items when `no_paginate` is true. |

> Parameters accept camelCase, snake_case, kebab-case, or CapitalCase variants.

---

## Example Response

```json
{
  "data": [
    {
      "id": 813,
      "event_time": "2025-08-17T00:44:24-03:00",
      "event": "page_view",
      "page": "https://example.com/my-messages",
      "title": "Messages",
      "session_id": "8e007f7f-b9bb-4376-b675-8ade018dfdd3",
      "ip_address": "203.0.113.10",
      "is_banned": 0,
      "is_robot": 0,
      "platform": {
        "uuid": "00000000-0000-0000-0000-000000000000",
        "name": "Example Platform"
      },
      "user": {
        "uuid": "11111111-1111-1111-1111-111111111111",
        "name": "John Doe"
      },
      "created_at": "2025-08-16T21:44:24-03:00"
    }
  ],
  "links": {
    "first": "https://sandbox.echosistema.online/api/v1/footprints?page=1",
    "last": "https://sandbox.echosistema.online/api/v1/footprints?page=32",
    "prev": null,
    "next": "https://sandbox.echosistema.online/api/v1/footprints?page=2"
  },
  "meta": {
    "current_page": 1,
    "from": 1,
    "last_page": 32,
    "path": "https://sandbox.echosistema.online/api/v1/footprints",
    "per_page": 25,
    "to": 1,
    "total": 798
  }
}
```

---

## JSON Structure Explanation

| Field | Type | Description |
| ----- | ---- | ----------- |
| `data[]` | `array` | List of footprints. |
| `data[].id` | `integer` | Footprint ID. |
| `data[].event_time` | `datetime` | Timestamp of the event. |
| `data[].event` | `string` | Event type. |
| `data[].page` | `string` | Page URL. |
| `data[].title` | `string` | Page title. |
| `data[].session_id` | `string` | Session identifier. |
| `data[].ip_address` | `ip` | Visitor IP address. |
| `data[].is_banned` | `boolean` | Indicates banned IP. |
| `data[].is_robot` | `boolean` | Indicates robot access. |
| `data[].platform` | `object` | Platform summary. |
| `data[].platform.uuid` | `uuid` | Platform UUID. |
| `data[].platform.name` | `string` | Platform name. |
| `data[].user` | `object` | User summary. |
| `data[].user.uuid` | `uuid` | User UUID. |
| `data[].user.name` | `string` | User name. |
| `data[].created_at` | `datetime` | Record creation time. |
| `links` | `object` | Pagination links. |
| `meta` | `object` | Pagination metadata. |

---

## Notes

* Hidden footprints are returned only to master users.
* Pagination metadata is omitted when `no_paginate` is truthy.
* Localisation: translatable fields follow `Accept-Language` when available; otherwise defaults are used.
