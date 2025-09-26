# Real Estate — Properties: List

## Endpoint

```
GET /real-estate/properties
```

## Authentication

Not required. Pass the public platform key via `public_key` when you need to scope the listing.

## Headers

| Header          | Type   | Required | Description |
| --------------- | ------ | -------- | ----------- |
| Accept-Language | string | No       | Locale used to localize textual fields (`pt-BR`, `en`, `es`, …). |

## Query Parameters

### Pagination & Sorting

| Parameter  | Type   | Required | Description | Default |
| ---------- | ------ | -------- | ----------- | ------- |
| page       | int    | No       | Page to return. | 1 |
| per_page   | int    | No       | Page size (1–200). | 9 |
| sort_by    | string | No       | Field alias for ordering. Supports `created_at`, `size`, `rooms`, `bed`, `bath`, `garage`, `is_featured`, `direct_from_owner`, `value`. | `createdAt` |
| direction  | string | No       | `asc` or `desc` (case-insensitive). | `DESC` |

### Identifiers & Ownership

| Parameter     | Type        | Required | Description |
| ------------- | ----------- | -------- | ----------- |
| public_key[]  | string      | No       | Filter by one or more public keys (`pbk`). |
| uuid[]        | uuid        | No       | Filter by property UUID(s). |
| agency_uuid[] | uuid        | No       | Filter by agency UUID(s). |
| agent_uuid[]  | uuid        | No       | Filter by agent UUID(s). |

### Location & Typing

| Parameter      | Type   | Required | Description |
| -------------- | ------ | -------- | ----------- |
| city[]         | string | No       | Filter by city name. |
| region[]       | string | No       | Filter by state/region. |
| country[]      | string | No       | Filter by ISO country code. |
| rent_type[]    | string | No       | Filter by rent type slug/value. |
| property_type[]| string | No       | Filter by property type slug/value. |

### Capacity & Amenities

| Parameter | Type  | Required | Description |
| --------- | ----- | -------- | ----------- |
| rooms     | int   | No       | Exact room count. |
| rooms_min | int   | No       | Minimum rooms. |
| rooms_max | int   | No       | Maximum rooms. |
| bed / bed_min / bed_max | int | No | Bedroom filters. |
| bath / bath_min / bath_max | int | No | Bathroom filters. |
| size_min  | int   | No       | Minimum size (sqft/m² based on dataset). |
| size_max  | int   | No       | Maximum size. |
| garage    | int   | No       | Exact garage slots. |

### Highlight & Pricing

| Parameter           | Type    | Required | Description |
| ------------------- | ------- | -------- | ----------- |
| is_featured         | boolean | No       | Filter featured properties. |
| direct_from_owner   | boolean | No       | Filter exclusive owner listings. |
| search              | string  | No       | Full-text search across metadata. |
| value_currency      | string  | No       | Currency code (`USD`, `EUR`, …). |
| value_min / value_max | number | No     | Value range filter. |

### Date Filters

| Parameter                | Type   | Required | Description |
| ------------------------ | ------ | -------- | ----------- |
| created_at               | date   | No       | Exact creation date. |
| created_at_period[from]  | date   | No       | Start date (aliases: `start`, `0`). |
| created_at_period[to]    | date   | No       | End date (aliases: `end`, `1`). |

> Array-style filters accept either repeated query parameters (`?uuid[]=...&uuid[]=...`) or dot-notation indices (`uuid[0]=...`).

## Example

### Request

```bash
curl -X GET \
  "https://api.example.com/real-estate/properties?public_key=realestate-demo&city=Lisbon&per_page=9"
```

### Response (200)

```json
{
  "current_page": 1,
  "data": [
    {
      "uuid": "e2c9c1c6-4cac-4c41-8f8e-6fd79d303001",
      "title": "Penthouse with River View",
      "city": "Lisbon",
      "region": "Lisbon",
      "country": "PT",
      "value": {
        "amount": 875000,
        "currency": "EUR"
      },
      "rentType": "sale",
      "propertyType": "penthouse",
      "rooms": 7,
      "bed": 4,
      "bath": 3,
      "garage": 2,
      "size": 215,
      "isFeatured": true,
      "directFromOwner": false,
      "createdAt": "2025-08-13T12:45:11Z"
    }
  ],
  "first_page_url": "https://api.example.com/real-estate/properties?page=1",
  "from": 1,
  "last_page": 12,
  "last_page_url": "https://api.example.com/real-estate/properties?page=12",
  "links": [],
  "next_page_url": "https://api.example.com/real-estate/properties?page=2",
  "path": "https://api.example.com/real-estate/properties",
  "per_page": 9,
  "prev_page_url": null,
  "to": 9,
  "total": 108
}
```

## Status Codes

- 200 — Success
- 400 — Validation error on filters
- 404 — Platform key not found
- 500 — Unexpected error

## Notes

- Results are automatically scoped to the provided `public_key` when present.
- Sorting accepts camelCase or snake_case aliases as shown above.
- The paginator uses Laravel's default structure (`LengthAwarePaginator`).

## Changelog

- 2025-09-25 — Initial documentation.
