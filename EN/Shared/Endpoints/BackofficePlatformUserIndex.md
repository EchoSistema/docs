# Shared — List Platform Users (Backoffice)

## Endpoint

```
GET /api/v1/backoffice/users
```

## Authentication

Required – Bearer {token} with `index.all` permission

## Headers

| Header            | Type     | Required    | Description |
| ----------------- | -------- | ----------- | ----------- |
| Authorization     | string   | Yes         | `Bearer {token}` credential. |
| X-PUBLIC-KEY      | string   | Yes         | Platform public key. |
| Accept-Language   | string   | No          | IETF locale (e.g., `pt-BR`, `en`, `es`). |

## Parameters

### Query parameters

| Parameter     | Type    | Required    | Description | Default/Values |
| ------------- | ------- | ----------- | ----------- | -------------- |
| per_page      | integer | No          | Number of items per page. | `25` |
| page          | integer | No          | Current page (when paginated). | `1` |
| no_paginate   | boolean | No          | If `true`, returns all records without pagination. | `false` |

> Parameters accept variants in `snake_case`, `camelCase`, and `kebab-case`.

## Examples

### Request example (curl)

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: en" \
  "https://sandbox.your-domain.com/api/v1/backoffice/users?per_page=25&page=1"
```

### Response example (200 - Paginated)

```json
{
  "data": [
    {
      "id": 1234,
      "echo_uuid": "echo-550e8400-e29b-41d4-a716",
      "name": "Maria Silva",
      "gender": {
        "symbol": "F",
        "name": "Female"
      },
      "age": 32,
      "birth_date": "1992-05-15T00:00:00+00:00",
      "email": "maria.silva@example.com",
      "avatar": "https://cdn.example.com/avatars/maria.webp",
      "created_at": "2024-01-15T10:30:00+00:00",
      "roles": [
        {
          "id": 2,
          "main": true,
          "platform": "Echo Education",
          "platform_uuid": "75f508e7-83ba-451c-9c2a-3df2aaf9db11",
          "domain": "Education",
          "role": "Admin",
          "language": "en",
          "currency": "USD",
          "status": "active",
          "created_at": "2024-01-15T10:35:00+00:00"
        }
      ]
    }
  ],
  "links": {
    "first": "https://api.example.com/api/v1/backoffice/users?page=1",
    "last": "https://api.example.com/api/v1/backoffice/users?page=10",
    "prev": null,
    "next": "https://api.example.com/api/v1/backoffice/users?page=2"
  },
  "meta": {
    "current_page": 1,
    "from": 1,
    "last_page": 10,
    "path": "https://api.example.com/api/v1/backoffice/users",
    "per_page": 25,
    "to": 25,
    "total": 250
  }
}
```

### Response example (200 - Without pagination)

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  "https://sandbox.your-domain.com/api/v1/backoffice/users?no_paginate=true"
```

```json
{
  "data": [
    {
      "id": 1234,
      "echo_uuid": "echo-550e8400-e29b-41d4-a716",
      "name": "Maria Silva",
      "gender": {
        "symbol": "F",
        "name": "Female"
      },
      "age": 32,
      "birth_date": "1992-05-15T00:00:00+00:00",
      "email": "maria.silva@example.com",
      "avatar": "https://cdn.example.com/avatars/maria.webp",
      "created_at": "2024-01-15T10:30:00+00:00",
      "roles": [...]
    }
  ]
}
```

## JSON Structure Explained

| Field                   | Type     | Description |
| ----------------------- | -------- | ----------- |
| data[]                  | array    | List of users |
| data[].id               | integer  | Internal user ID |
| data[].echo_uuid        | string   | Echo unique identifier for the user |
| data[].name             | string   | User's full name |
| data[].gender           | object   | Gender information object |
| data[].gender.symbol    | string   | Gender symbol (e.g., `M`, `F`) |
| data[].gender.name      | string   | Translated gender name |
| data[].age              | integer  | Calculated user age |
| data[].birth_date       | string   | Birth date (ISO 8601) |
| data[].email            | string   | User's email |
| data[].avatar           | string   | User's avatar URL |
| data[].created_at       | string   | Creation date (ISO 8601) |
| data[].roles[]          | array    | List of user roles in platforms |
| data[].roles[].id       | integer  | Role ID |
| data[].roles[].main     | boolean  | Whether it's the user's main platform |
| data[].roles[].platform | string   | Platform name |
| data[].roles[].platform_uuid | string | Platform UUID |
| data[].roles[].domain   | string   | Platform's domain area |
| data[].roles[].role     | string   | Role/position name |
| data[].roles[].language | string   | Platform language (e.g., `en`, `pt-BR`) |
| data[].roles[].currency | string   | Platform currency (e.g., `BRL`, `USD`) |
| data[].roles[].status   | string   | Role status (e.g., `active`, `inactive`) |
| data[].roles[].created_at | string | Role creation date (ISO 8601) |
| links                   | object   | Pagination links (when paginated) |
| meta                    | object   | Pagination metadata (when paginated) |

**Note about typo:** There is a typo in the code (`staus` instead of `status`) in `BackofficeUserResource.php:38`. The returned field is `staus`.

## HTTP Status

- 200: Success
- 401: Unauthenticated
- 403: Forbidden (no `index.all` permission)
- 429: Too many requests
- 500: Internal error

## Errors

### 403 Forbidden
User does not have the required permission:

```json
{
  "message": "Forbidden"
}
```

### 401 Unauthorized
Missing or invalid token:

```json
{
  "message": "Unauthenticated."
}
```

## Notes

- This endpoint returns users with eager loading of: `regionalInformation`, `identities`, `telephone`, `avatarImage`, `profileImage`, `platform`, `platformRoles`, `platformRoles.platform`
- The response uses `BackofficeUserCollection` and `BackofficeUserResource` for formatting
- When `is_index=true` (default for this endpoint), extra fields like `updated_at`, `language`, `currency`, and `telephone` from the user are **not** included in the response (see `BackofficeUserResource.php:41-48`)
- The `no_paginate=true` parameter should be used carefully in environments with many users
- The `main` field in `roles[]` indicates whether that platform is the user's main platform
- There is a typo in the Resource: the returned field is `staus` instead of `status` (line 38 of BackofficeUserResource.php)

## Related

- [List Platforms](./BackofficePlatformIndex.md)
- [List Domain Areas](./BackofficeDomainAreaIndex.md)
- [List Roles](./RoleIndex.md)

## Changelog

- 2025-10-05: Complete refactoring of documentation based on actual code (BackofficeUserResource and BackofficeUserCollection)
- 2025-10-04: Initial document
