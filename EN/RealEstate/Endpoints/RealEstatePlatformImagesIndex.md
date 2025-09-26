# Real Estate — Platform Images: List

## Endpoint

```
GET /real-estate/platform-images
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
curl -X GET "https://api.example.com/real-estate/platform-images?public_key=realestate-demo"
```

### Response (200)

```json
{
  "data": [
    {
      "base": "card",
      "data": {
        "url": "https://cdn.example.com/real-estate/card-default.png",
        "alt": "Hero image"
      },
      "pbk": "realestate-demo"
    }
  ],
  "meta": {
    "total": 1
  }
}
```

## Status Codes

- 200 — Success
- 404 — Platform key not found

## Notes

- Images are stored as flexible structures (usually URL + metadata) in MongoDB collection `platform_images`.
- All entries for the supplied `public_key` are returned in a single payload.

## Changelog

- 2025-09-25 — Initial documentation.
