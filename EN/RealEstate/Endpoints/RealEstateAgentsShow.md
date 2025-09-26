# Real Estate — Agents: Detail

## Endpoint

```
GET /real-estate/agents/{agent}
```

## Authentication

Not required.

## Path Parameters

| Parameter | Type | Required | Description |
| --------- | ---- | -------- | ----------- |
| agent     | uuid | Yes      | Agent UUID (`agents.uuid`). |

## Example

### Request

```bash
curl -X GET "https://api.example.com/real-estate/agents/0f7e3d9a-2a19-4e4b-b0a3-3fb2ef25d001"
```

### Response (200)

```json
{
  "data": {
    "uuid": "0f7e3d9a-2a19-4e4b-b0a3-3fb2ef25d001",
    "name": "Laura Nogueira",
    "email": "laura@agency.com",
    "avatar": "https://cdn.example.com/agents/laura.png",
    "phones": ["+55 11 99999-0001"],
    "masked_phones": ["+55 (11) •••••-0001"],
    "role": "senior-agent",
    "properties": 42,
    "socials": {
      "instagram": "@laura.moves",
      "linkedin": "laura-nogueira"
    }
  }
}
```

## Status Codes

- 200 — Success
- 404 — Agent not found

## Notes

- The document is retrieved from MongoDB (`agents`).
- No platform key is required because the UUID is unique per tenant.

## Changelog

- 2025-09-25 — Initial documentation.
