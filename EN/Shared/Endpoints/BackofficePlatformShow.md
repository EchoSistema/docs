# Shared – Show Platform Details

## Endpoint

```
GET /api/v1/backoffice/platforms/{platform}
```

## Authentication

Required – Bearer {token} with ability `backoffice`

## Headers

| Header     | Type | Required | Description |
| ---------------- | ------ | -------- | ----------- |
| Authorization    | string | When required | `Bearer {token}`. |
| X-PUBLIC-KEY     | string | Yes      | Platform public key. |
| Accept-Language  | string | No       | IETF locale (e.g., `pt-BR`, `en`, `es`). |

## URL Parameters

| Parameter | Type | Required | Description |
| ----------- | ------- | ----------- | ----------- |
| platform    | string  | Yes         | Platform UUID to retrieve |

## Examples

### Request example (curl)

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: en" \
  "https://sandbox.your-domain.com/api/v1/backoffice/platforms/9d4e1c2a-3b4c-5d6e-7f8g-9h0i1j2k3l4m"
```

### Response example

```json
{
  "data": {
    "uuid": "9d4e1c2a-3b4c-5d6e-7f8g-9h0i1j2k3l4m",
    "total": {
      "users": 150
    },
    "domain": {
      "uuid": "8c3d0b1a-2e3f-4g5h-6i7j-8k9l0m1n2o3p",
      "name": "E-commerce"
    },
    "name": "Demo Platform",
    "email": "contact@platform.com",
    "open_to_register": true,
    "public_key": "pk_test_123456789",
    "logo": {
      "main": "https://cdn.example.com/logos/main.png",
      "square": "https://cdn.example.com/logos/square.png",
      "rounded": "https://cdn.example.com/logos/rounded.png",
      "rectangular": "https://cdn.example.com/logos/rectangular.png"
    },
    "created_at": "2024-01-15T10:30:00Z"
  }
}
```

## JSON Structure Explained

| Field | Type | Description |
| ----------- | ------- | ----------- |
| data        | object  | Platform data |
| data.uuid   | string  | Platform unique identifier |
| data.total  | object  | Platform totals |
| data.total.users | integer | Total number of users on the platform |
| data.domain | object  | Domain area information |
| data.domain.uuid | string  | Domain area unique identifier |
| data.domain.name | string  | Domain area name |
| data.name   | string  | Platform name |
| data.email  | string  | Platform contact email |
| data.open_to_register | boolean | Indicates if the platform is open for new registrations |
| data.public_key | string  | Platform public key |
| data.logo   | object  | Platform logo URLs |
| data.logo.main | string  | Main logo URL |
| data.logo.square | string  | Square logo URL |
| data.logo.rounded | string  | Rounded logo URL |
| data.logo.rectangular | string  | Rectangular logo URL |
| data.created_at | string  | Creation date (ISO 8601) |

## HTTP Status

- 200: OK
- 201: Created
- 400: Bad Request
- 401: Unauthorized
- 403: Forbidden
- 404: Not Found
- 422: Unprocessable Entity
- 429: Too Many Requests
- 500: Internal Server Error

## Errors

```json
{
  "message": "Error message"
}
```

## Notes

- This endpoint returns the details of a specific platform
- Only users with backoffice permission can access
- Includes platform user count (users_count)
- The {platform} parameter accepts the platform UUID

## Related

- [List Platforms](BackofficePlatformIndex.md)
- [List Platform Users](BackofficePlatformUserIndex.md)

## Changelog

- 2025-10-25: Complete documentation update with detailed structure
- 2025-10-16: Initial documentation
