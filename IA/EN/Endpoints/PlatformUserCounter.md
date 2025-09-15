# Platform Users — Counter

- Method: `GET`
- Route: `/api/v1/ia/admin/users/counter`
- Auth: `auth:sanctum`

## Summary
Returns the amount of users per role within the current platform, respecting the requester’s minimum role visibility.

## Query Params
- Same filters as Index (role/user/occupation) to narrow the counting scope.

## Response

```json
{
  "data": {
    "counter": {
      "guest": {
        "role_id": 5,
        "name": "guest",
        "localized_name": "Guest",
        "total": 42
      }
    }
  }
}
```

