# Shared – List Available Roles

## Endpoint

```
GET /api/v1/roles
```

## Authentication

None

## Headers

| Header     | Type | Required | Description |
| ---------------- | ------ | -------- | ----------- |
| Authorization    | string | No | `Bearer {token}`. |
| X-PUBLIC-KEY     | string | Yes      | Platform public key. |
| Accept-Language  | string | No       | IETF locale (e.g., `pt-BR`, `en`, `es`). |

## Query Parameters

| Parameter | Type | Required | Description |
| --------- | ---- | -------- | ----------- |
| roles | array | No | Array of specific role names to include (e.g., `administrator`, `supervisor`) |
| roles.* | enum | No | Must be valid RoleEnum values |
| except | array | No | Array of role names to exclude from the response |
| counting | array | No | Array of relationships to count (e.g., `["users", "platforms"]`) |
| permissions | boolean | No | Include role permissions in response (default: false) |

## Examples

### Request example 1: Basic (curl)

```bash
curl -X GET \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: en" \
  "https://sandbox.your-domain.com/api/v1/roles"
```

### Request example 2: With counting (curl)

```bash
curl -X GET \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: en" \
  "https://sandbox.your-domain.com/api/v1/roles?counting[]=users&counting[]=platforms&permissions=1"
```

### Request example 3: Filter specific roles (curl)

```bash
curl -X GET \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: en" \
  "https://sandbox.your-domain.com/api/v1/roles?roles[]=administrator&roles[]=supervisor&except[]=guest"
```

### Response example (basic)

```json
{
  "data": [
    {
      "id": 1,
      "uuid": "550e8400-e29b-41d4-a716-446655440000",
      "name": "administrator",
      "title": "Administrator",
      "created_at": "2024-01-15T10:30:00.000000Z"
    },
    {
      "id": 2,
      "uuid": "660e8400-e29b-41d4-a716-446655440001",
      "name": "supervisor",
      "title": "Supervisor",
      "created_at": "2024-01-15T10:30:00.000000Z"
    }
  ]
}
```

### Response example (with counting and permissions)

```json
{
  "data": [
    {
      "id": 1,
      "uuid": "550e8400-e29b-41d4-a716-446655440000",
      "name": "administrator",
      "title": "Administrator",
      "created_at": "2024-01-15T10:30:00.000000Z",
      "users_count": 150,
      "platforms_count": 12,
      "permissions": [
        "backoffice.all",
        "platform.manage",
        "users.manage"
      ]
    }
  ]
}
```

## JSON Structure Explained

| Field | Type | Description |
| ----------- | ------- | ----------- |
| data | array | Array of role objects |
| data[].id | integer | Role unique identifier |
| data[].uuid | string | Role UUID |
| data[].name | string | Role system name |
| data[].title | string | Localized role display name |
| data[].created_at | string | ISO 8601 timestamp |
| data[].users_count | integer | Count of users with this role (only when `counting` includes "users") |
| data[].platforms_count | integer | Count of platforms using this role (only when `counting` includes "platforms") |
| data[].permissions | array | Array of permission strings (only when `permissions=1`) |

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

- The endpoint returns default roles: `administrator`, `supervisor`, `coordinator`, `support`, `guest`
- Additional roles can be requested via the `roles` parameter
- Specific roles can be excluded using the `except` parameter
- The `counting` parameter enables relationship counts (users, platforms)
- Domain-specific behavior: ReputationBook and Intelligence domains support `withCount`, others return all roles
- Use `permissions=1` to include the full list of permissions for each role

## Related

- See other Shared API endpoints
- Role permissions are defined in `Domain\Shared\Enums\Role\RoleEnum`

## Changelog

- 2025-11-20: Added counting, filtering parameters and detailed documentation
- 2025-10-16: Initial documentation
