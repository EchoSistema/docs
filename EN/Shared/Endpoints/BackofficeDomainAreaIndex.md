# Shared – List Domain Areas (Backoffice)

## Endpoint

```
GET /api/v1/backoffice/domain-areas
```

## Authentication

Required – Bearer {token} with `index.all` permission

## Headers

| Header            | Type     | Required    | Description |
| ----------------- | -------- | ----------- | ----------- |
| Authorization     | string   | Yes         | `Bearer {token}` credential. |
| X-PUBLIC-KEY      | string   | Yes         | Platform public key. |

## Parameters

This endpoint does not accept path or query parameters.

## Examples

### Request example (curl)

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  "https://sandbox.your-domain.com/api/v1/backoffice/domain-areas"
```

### Response example

```json
{
  "data": [
    {
      "uuid": "550e8400-e29b-41d4-a716-446655440000",
      "name": "Biography",
      "slug": "bio",
      "is_active": true
    },
    {
      "uuid": "660e8400-e29b-41d4-a716-446655440001",
      "name": "Healthcare",
      "slug": "healthcare",
      "is_active": true
    }
  ]
}
```

## JSON Structure Explained

| Field           | Type     | Description |
| --------------- | -------- | ----------- |
| data[]          | array    | List of domain areas available in the system |
| data[].uuid     | string   | Universal unique identifier of the domain area |
| data[].name     | string   | Domain area name |
| data[].slug     | string   | Domain area slug (unique textual identifier) |
| data[].is_active| boolean  | Indicates if the domain area is active in the system |

## HTTP Status

- 200: Success
- 401: Unauthenticated
- 403: Forbidden (no `index.all` permission)
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

- This endpoint returns all domain areas registered in the system, including inactive ones
- Domain areas are used to organize platforms, categories, and item types
- The `slug` field can be used to identify specific domain areas programmatically
- Permission check is performed through the controller's `checkPermissions('index.all')` method
- Only the fields `uuid`, `name`, `slug`, and `is_active` are returned (explicitly selected fields)

## Related

- [List Platforms](./BackofficePlatformIndex.md)
- [List Categories by Domain](./CategoryGroupIndex.md)

## Changelog

- 2025-10-05: Initial documentation created
