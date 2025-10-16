# RealEstate – List Real Estate Agents

## Endpoint

```
GET /api/v1/real-estate/agents
```

## Authentication

None

## Headers

| Header           | Type   | Required | Description |
| ---------------- | ------ | -------- | ----------- |
| X-PUBLIC-KEY     | string | Yes      | Platform public key. |
| Accept-Language  | string | No       | IETF locale (e.g., `pt-BR`, `en`, `es`). |

## Parameters

### Query parameters

| Parameter | Type    | Required | Description | Default/Values |
| --------- | ------- | -------- | ----------- | -------------- |
| per_page | integer | No | Items per page | 9 (max: 200) |
| page | integer | No | Page number | 1 |

## Examples

### Request example (curl)

```bash
curl -X GET \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: en" \
  "https://sandbox.your-domain.com/api/v1/real-estate/agents?per_page=9"
```

### Response example

```json
{
  "data": [
    {
      "uuid": "660e8400-e29b-41d4-a716-446655440001",
      "name": "John Smith",
      "email": "john.smith@agency.com",
      "phone": "+1-555-0123",
      "bio": "Experienced real estate agent with over 10 years in the Miami market",
      "agency": {
        "uuid": "770e8400-e29b-41d4-a716-446655440002",
        "name": "Premium Real Estate"
      },
      "avatar": "https://example.com/agents/john-smith.jpg",
      "properties_count": 25,
      "created_at": "2024-01-15T08:00:00Z"
    }
  ],
  "links": {
    "first": "https://sandbox.your-domain.com/api/v1/real-estate/agents?page=1",
    "last": "https://sandbox.your-domain.com/api/v1/real-estate/agents?page=3",
    "prev": null,
    "next": "https://sandbox.your-domain.com/api/v1/real-estate/agents?page=2"
  },
  "meta": {
    "current_page": 1,
    "from": 1,
    "last_page": 3,
    "per_page": 9,
    "to": 9,
    "total": 25
  }
}
```

## JSON Structure Explained

| Field       | Type    | Description |
| ----------- | ------- | ----------- |
| data[] | array | List of real estate agents |
| data[].uuid | string | Agent unique identifier |
| data[].name | string | Agent full name |
| data[].email | string | Agent email address |
| data[].phone | string | Agent phone number |
| data[].bio | string | Agent biography/description |
| data[].agency | object | Agency information |
| data[].agency.uuid | string | Agency unique identifier |
| data[].agency.name | string | Agency name |
| data[].avatar | string | Agent profile picture URL |
| data[].properties_count | integer | Number of properties managed by agent |
| data[].created_at | string | Agent registration timestamp (ISO 8601) |
| links | object | Pagination links |
| meta | object | Pagination metadata |

## HTTP Status

- 200: OK
- 400: Bad Request
- 422: Unprocessable Entity
- 429: Too Many Requests
- 500: Internal Server Error

## Errors

```json
{
  "message": "Validation error",
  "errors": {
    "per_page": ["The per page field must not be greater than 200."]
  }
}
```

## Notes

- The `public_key` parameter is automatically extracted from the `X-PUBLIC-KEY` header
- Results are paginated with a default of 9 items per page (max 200)
- Only agents associated with the platform are returned
- The `properties_count` field shows active properties for each agent

## Related

- [Show Agent Details](AgentShow.md)
- [List Properties](PropertyIndex.md)

## Changelog

- 2025-10-16: Initial documentation
