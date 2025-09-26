# Real Estate — Rental Property Types: List

## Endpoint

```
GET /real-estate/rental-properties/types
```

## Authentication

Not required.

## Headers

| Header          | Type   | Required | Description |
| --------------- | ------ | -------- | ----------- |
| Accept-Language | string | No       | Matches the requested language for type titles. |

## Query Parameters

| Parameter | Type   | Required | Description |
| --------- | ------ | -------- | ----------- |
| language  | string | No       | Locale to retrieve titles (fallbacks to default when missing). |

## Example

### Request

```bash
curl -X GET "https://api.example.com/real-estate/rental-properties/types?language=en"
```

### Response (200)

```json
{
  "data": [
    {
      "uuid": "c1927cd4-40c1-4d92-ab2e-4c05bb560001",
      "slug": "apartment",
      "name": "Apartment"
    },
    {
      "uuid": "f0278cbe-2665-42c3-94c3-05c4371c0002",
      "slug": "loft",
      "name": "Loft"
    }
  ],
  "meta": {
    "domainArea": {
      "uuid": "df742b1d-1be8-4c73-9c5f-3c7076000101",
      "name": "Real Estate"
    }
  }
}
```

## Status Codes

- 200 — Success
- 404 — Domain area `realestate` not configured

## Notes

- The endpoint loads the domain area `realestate` and returns its associated item types, ensuring titles match the requested language or the default fallback.
- When the domain area cannot be found, a 404 with the translated `common.not_found` message is returned.

## Changelog

- 2025-09-25 — Initial documentation.
