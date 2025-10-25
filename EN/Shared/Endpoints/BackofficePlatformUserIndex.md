# Shared – List All Users

## Endpoint

```
GET /api/v1/backoffice/users
```

## Authentication

Required – Bearer {token} with `backoffice` ability

## Headers

| Header        | Type   | Required | Description |
| ---------------- | ------ | -------- | ----------- |
| Authorization    | string | When required | `Bearer {token}` credential. |
| X-PUBLIC-KEY     | string | Yes      | Platform public key. |
| Accept-Language  | string | No       | IETF locale (e.g., `pt-BR`, `en`, `es`). |

## Query Parameters

| Parameter   | Type    | Required | Description |
| ----------- | ------- | -------- | ----------- |
| no_paginate | boolean | No       | If `true`, returns all results without pagination. |
| per_page    | integer | No       | Number of records per page (default: 25). |

## Examples

### Request example (curl)

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: en" \
  "https://sandbox.your-domain.com/api/v1/backoffice/users?per_page=25"
```

### Response example

```json
{
  "data": [
    {
      "id": 1234,
      "echo_uuid": "echo_9d4e1c2a3b4c5d6e",
      "uuid": "9d4e1c2a-3b4c-5d6e-7f8g-9h0i1j2k3l4m",
      "name": "John Doe",
      "gender": {
        "symbol": "M",
        "name": "Male"
      },
      "age": 32,
      "birth_date": "1992-05-15T00:00:00Z",
      "email": "john.doe@example.com",
      "avatar": "https://cdn.example.com/avatars/john-doe.jpg",
      "created_at": "2024-01-15T10:30:00Z",
      "roles": [
        {
          "id": 5,
          "main": true,
          "platform": "E-commerce Platform",
          "platform_uuid": "8c3d0b1a-2e3f-4g5h-6i7j-8k9l0m1n2o3p",
          "domain": "E-commerce",
          "role": "Admin",
          "language": "en",
          "currency": "USD",
          "status": "active",
          "created_at": "2024-01-15T10:30:00Z"
        },
        {
          "id": 7,
          "main": false,
          "platform": "Articles Platform",
          "platform_uuid": "7b2c9a0b-1d2e-3f4g-5h6i-7j8k9l0m1n2o",
          "domain": "Articles",
          "role": "Editor",
          "language": "en",
          "currency": "USD",
          "status": "active",
          "created_at": "2024-02-20T14:15:00Z"
        }
      ]
    },
    {
      "id": 1235,
      "echo_uuid": "echo_8c3d0b1a2e3f4g5h",
      "uuid": "8c3d0b1a-2e3f-4g5h-6i7j-8k9l0m1n2o3p",
      "name": "Jane Smith",
      "gender": {
        "symbol": "F",
        "name": "Female"
      },
      "age": 28,
      "birth_date": "1996-08-22T00:00:00Z",
      "email": "jane.smith@example.com",
      "avatar": "https://cdn.example.com/avatars/jane-smith.jpg",
      "created_at": "2024-01-20T15:45:00Z",
      "roles": [
        {
          "id": 3,
          "main": true,
          "platform": "RealEstate Platform",
          "platform_uuid": "6a1b8c9d-0e1f-2g3h-4i5j-6k7l8m9n0o1p",
          "domain": "RealEstate",
          "role": "Agent",
          "language": "en",
          "currency": "USD",
          "status": "active",
          "created_at": "2024-01-20T15:45:00Z"
        }
      ]
    }
  ],
  "links": {
    "first": "https://sandbox.your-domain.com/api/v1/backoffice/users?page=1",
    "last": "https://sandbox.your-domain.com/api/v1/backoffice/users?page=10",
    "prev": null,
    "next": "https://sandbox.your-domain.com/api/v1/backoffice/users?page=2"
  },
  "meta": {
    "current_page": 1,
    "from": 1,
    "last_page": 10,
    "per_page": 25,
    "to": 25,
    "total": 250
  }
}
```

## JSON Structure Explained

| Field | Type | Description |
| ----------- | ------- | ----------- |
| data        | array   | List of users |
| data[].id   | integer | Internal user ID |
| data[].echo_uuid | string  | EchoSistema UUID (format: echo_*) |
| data[].uuid | string  | Unique user identifier |
| data[].name | string  | User's full name |
| data[].gender | object  | Gender information |
| data[].gender.symbol | string  | Gender symbol (M, F, O) |
| data[].gender.name | string  | Gender name in full |
| data[].age  | integer | Calculated user age |
| data[].birth_date | string  | Birth date (ISO 8601) |
| data[].email | string  | User's email |
| data[].avatar | string  | Avatar image URL |
| data[].created_at | string  | Account creation date (ISO 8601) |
| data[].roles | array   | List of user roles/functions across different platforms |
| data[].roles[].id | integer | Role ID |
| data[].roles[].main | boolean | Indicates if this is the user's main platform |
| data[].roles[].platform | string  | Platform name |
| data[].roles[].platform_uuid | string  | Platform UUID |
| data[].roles[].domain | string  | Platform domain area |
| data[].roles[].role | string  | Role/function name |
| data[].roles[].language | string  | Configured language (IETF locale) |
| data[].roles[].currency | string  | Configured currency (ISO code) |
| data[].roles[].status | string  | Role status (active, inactive, etc.) |
| data[].roles[].created_at | string  | Role assignment date (ISO 8601) |
| links       | object  | Pagination links |
| meta        | object  | Pagination metadata |

## HTTP Status

- 200: OK
- 201: Created
- 400: Bad request
- 401: Unauthorized
- 403: Forbidden
- 404: Not found
- 422: Validation error
- 429: Too many requests
- 500: Internal error

## Errors

```json
{
  "message": "Error message"
}
```

## Notes

- This endpoint returns all users from all platforms in the system
- Only users with `index.all` backoffice permission can access
- The response includes the following loaded relationships:
  - `regionalInformation`: User's regional information (language, currency)
  - `identities`: User's identity documents
  - `telephone`: Primary phone number
  - `avatarImage`: Avatar image
  - `profileImage`: Profile image
  - `platform`: User's main platform
  - `platformRoles`: All user roles across different platforms
  - `platformRoles.platform`: Platform data associated with roles
- The `avatar` field is dynamically generated through the `getAvatar()` method
- The `age` field is automatically calculated from birth date
- The `roles` field consolidates all user roles with main platform indication
- Default sorted by creation date

## Related

- [Show User Details](BackofficePlatformUserShow.md)
- [List Platforms](BackofficePlatformIndex.md)

## Changelog

- 2025-10-25: Complete documentation update with detailed structure and all fields
- 2025-10-16: Initial documentation
