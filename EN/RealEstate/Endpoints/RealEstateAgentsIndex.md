# Real Estate — Agents: List

## Endpoint

```
GET /real-estate/agents
```

## Authentication

Not required.

## Headers

| Header          | Type   | Required | Description |
| --------------- | ------ | -------- | ----------- |
| Accept-Language | string | No       | Localizes textual fields when available. |

## Query Parameters

| Parameter | Type   | Required | Description | Default |
| --------- | ------ | -------- | ----------- | ------- |
| public_key | string | Yes      | Platform public key (`pbk`). |
| per_page   | int    | No       | Page size (1–200). | 9 |
| page       | int    | No       | Page index. | 1 |

## Example

### Request

```bash
curl -X GET "https://api.example.com/real-estate/agents?public_key=realestate-demo&per_page=9"
```

### Response (200)

```json
{
  "current_page": 1,
  "data": [
    {
      "uuid": "0f7e3d9a-2a19-4e4b-b0a3-3fb2ef25d001",
      "name": "Laura Nogueira",
      "email": "laura@agency.com",
      "avatar": "https://cdn.example.com/agents/laura.png",
      "phones": ["+55 11 99999-0001"],
      "masked_phones": ["+55 (11) •••••-0001"],
      "role": "senior-agent",
      "properties": 42
    }
  ],
  "first_page_url": "https://api.example.com/real-estate/agents?page=1",
  "from": 1,
  "last_page": 5,
  "last_page_url": "https://api.example.com/real-estate/agents?page=5",
  "links": [],
  "next_page_url": "https://api.example.com/real-estate/agents?page=2",
  "path": "https://api.example.com/real-estate/agents",
  "per_page": 9,
  "prev_page_url": null,
  "to": 9,
  "total": 42
}
```

## Status Codes

- 200 — Success
- 400 — Invalid pagination parameters
- 404 — Platform key not found

## Notes

- Every result belongs to the provided `public_key`.
- Fields are stored in MongoDB and may vary per deployment.

## Changelog

- 2025-09-25 — Initial documentation.
