# Real Estate — Platform Texts: List

## Endpoint

```
GET /real-estate/platform-texts
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
curl -X GET "https://api.example.com/real-estate/platform-texts?public_key=realestate-demo"
```

### Response (200)

```json
{
  "data": [
    {
      "key": "hero.title",
      "text": "Find your next home",
      "pbk": "realestate-demo"
    },
    {
      "key": "hero.subtitle",
      "text": "Curated listings with verified agents",
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

- Text records live in MongoDB collection `platform_texts` and can store localized content per key.
- All entries for the provided `public_key` are returned at once.

## Changelog

- 2025-09-25 — Initial documentation.
