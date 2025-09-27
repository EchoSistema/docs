# ReputationBook – Platform-Linked Company

## Endpoint

```
GET /api/v1/complaint-book/me
```

> This route belongs to the backoffice group (`ability: backoffice,domain:reputationbook`). Although it shares the path with the public endpoint, authorization ensures only platform collaborators access this variant.

## Authentication

Required – `Bearer {token}` with abilities `backoffice` and `domain:reputationbook`.

## Query Parameters

| Parameter | Type | Required | Description |
| --------- | ---- | -------- | ----------- |
| `platform_id` | integer | Yes | Internal platform ID. |
| `rel` | string | No | Comma-separated list of extra relations to eager load (`addresses`, `contacts`, `documents`, ...). Values are camel-cased by the controller. |

## Example Response

```json
{
  "data": {
    "uuid": "8d0f47bf-5e40-4bb4-9dc1-1ae75da3f148",
    "name": "Consumer Protection",
    "slug": "consumer-protection",
    "is_governamental": true,
    "platform": {
      "email": "contact@platform.gov",
      "favicon": "https://cdn.assets/favicon.ico",
      "logo": {
        "main": "https://cdn.assets/logo.png",
        "square": "https://cdn.assets/logo-square.png",
        "rounded": "https://cdn.assets/logo-rounded.png",
        "rectangular": "https://cdn.assets/logo-rect.png"
      },
      "domain_area": {
        "uuid": "963e42f1-5c2e-4a86-8866-2e05fcf7f3f9",
        "name": "Consumer Protection"
      }
    },
    "addresses": [
      {
        "type": "headquarters",
        "zipcode": "70000-000",
        "address_one": "Esplanade, block A",
        "city": { "id": 1, "name": "Brasília" },
        "state": { "id": 7, "name": "Distrito Federal" },
        "country": { "id": 33, "name": "Brazil" }
      }
    ],
    "contacts": [
      {
        "id": 10,
        "type": "telephone",
        "country_code": "+55",
        "number": "6130000000",
        "full_number": "+556130000000"
      }
    ],
    "documents": [
      {
        "uuid": "b3d2e698-4a6e-41d3-9a50-288e7b2c506b",
        "type": "ruc",
        "value": "80012345-6",
        "expires_at": null
      }
    ],
    "coverages": [
      {
        "type": "city",
        "values": ["Encarnación", "Posadas"]
      }
    ]
  }
}
```

## HTTP Status Codes

- 200: Company linked to the platform returned.
- 400/422: Missing parameters (e.g., `platform_id`).
- 404: Platform has no associated company.

