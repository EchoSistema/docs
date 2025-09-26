# Real Estate — Rent Types: List

## Endpoint

```
GET /real-estate/rent-types
```

## Authentication

Not required.

## Query Parameters

| Parameter  | Type   | Required | Description |
| ---------- | ------ | -------- | ----------- |
| public_key | string | Yes      | Platform public key (`pbk`). |

## Example

### Request

```bash
curl -X GET "https://api.example.com/real-estate/rent-types?public_key=realestate-demo"
```

### Response (200)

```json
{
  "data": [
    {
      "title": "Daily",
      "value": "daily",
      "pbk": "realestate-demo"
    },
    {
      "title": "Monthly",
      "value": "monthly",
      "pbk": "realestate-demo"
    }
  ],
  "meta": {
    "total": 2
  }
}
```

## Status Codes

- 200 — Success
- 404 — Platform key not found

## Notes

- Fields come straight from MongoDB collection `rent_types`.
- There is no pagination; expect the full list per platform.

## Changelog

- 2025-09-25 — Initial documentation.
