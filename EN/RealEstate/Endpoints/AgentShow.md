# RealEstate – Show Agent Details

## Endpoint

```
GET /api/v1/real-estate/agents/{uuid}
```

## Authentication

None

## Headers

| Header           | Type   | Required | Description |
| ---------------- | ------ | -------- | ----------- |
| X-PUBLIC-KEY     | string | Yes      | Platform public key. |
| Accept-Language  | string | No       | IETF locale (e.g., `pt-BR`, `en`, `es`). |

## Parameters

### Path parameters

| Parameter | Type   | Required | Description |
| --------- | ------ | -------- | ----------- |
| uuid      | string | Yes      | Agent unique identifier (UUID format) |

## Examples

### Request example (curl)

```bash
curl -X GET \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: en" \
  "https://sandbox.your-domain.com/api/v1/real-estate/agents/660e8400-e29b-41d4-a716-446655440001"
```

### Response example

```json
{
  "data": {
    "uuid": "660e8400-e29b-41d4-a716-446655440001",
    "name": "John Smith",
    "email": "john.smith@agency.com",
    "phone": "+1-555-0123",
    "mobile": "+1-555-0124",
    "bio": "Experienced real estate agent with over 10 years in the Miami market. Specializing in luxury properties and waterfront homes.",
    "agency": {
      "uuid": "770e8400-e29b-41d4-a716-446655440002",
      "name": "Premium Real Estate",
      "address": "123 Main Street, Miami, FL",
      "website": "https://premiumrealestate.com"
    },
    "avatar": "https://example.com/agents/john-smith.jpg",
    "properties_count": 25,
    "specialties": [
      "Luxury Properties",
      "Waterfront Homes",
      "Commercial Real Estate"
    ],
    "languages": ["English", "Spanish"],
    "social_media": {
      "facebook": "https://facebook.com/johnsmith",
      "instagram": "https://instagram.com/johnsmith",
      "linkedin": "https://linkedin.com/in/johnsmith"
    },
    "created_at": "2024-01-15T08:00:00Z",
    "updated_at": "2025-10-16T10:30:00Z"
  }
}
```

## JSON Structure Explained

| Field       | Type    | Description |
| ----------- | ------- | ----------- |
| data | object | Agent details |
| data.uuid | string | Agent unique identifier |
| data.name | string | Agent full name |
| data.email | string | Agent email address |
| data.phone | string | Agent office phone number |
| data.mobile | string | Agent mobile phone number |
| data.bio | string | Agent biography/description |
| data.agency | object | Agency information |
| data.agency.uuid | string | Agency unique identifier |
| data.agency.name | string | Agency name |
| data.agency.address | string | Agency physical address |
| data.agency.website | string | Agency website URL |
| data.avatar | string | Agent profile picture URL |
| data.properties_count | integer | Number of active properties managed by agent |
| data.specialties[] | array | List of agent specialties |
| data.languages[] | array | Languages spoken by agent |
| data.social_media | object | Social media profile links |
| data.created_at | string | Agent registration timestamp (ISO 8601) |
| data.updated_at | string | Agent last update timestamp (ISO 8601) |

## HTTP Status

- 200: OK
- 404: Not Found
- 429: Too Many Requests
- 500: Internal Server Error

## Errors

```json
{
  "message": "Agent not found"
}
```

## Notes

- The agent must belong to the platform specified by the `X-PUBLIC-KEY` header
- All translatable fields respect the `Accept-Language` header
- Social media links are optional and may be null
- The `properties_count` includes only active/published properties

## Related

- [List Agents](AgentIndex.md)
- [List Properties](PropertyIndex.md)

## Changelog

- 2025-10-16: Initial documentation
