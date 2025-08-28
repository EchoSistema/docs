# Shared – Roles Index Endpoint

## Endpoint

`GET /api/v1/roles`

Lists available roles. Roles depend on the requesting platform's domain. Optional parameters allow including or excluding specific roles.

---

## Authentication

None.

---

## Request

### Query Parameters

| Parameter | Type | Required | Description |
| --------- | ---- | -------- | ----------- |
| `roles` | `array` | No | Include only these roles. Values must be valid role names. |
| `except` | `array` | No | Exclude these roles from the result. |
| `permissions` | `boolean` | No | When `true`, includes role permissions in the response. Defaults to `false`. |

> Parameters accept camelCase, snake_case, kebab-case, or CapitalCase variants.

---

## Example Response

```json
{
  "data": [
    {
      "id": 1,
      "uuid": "00000000-0000-0000-0000-000000000000",
      "name": "admin",
      "title": "Administrator",
      "permissions": ["role.read", "role.write"]
    }
  ]
}
```

---

## JSON Structure Explanation

| Field | Type | Description |
| ----- | ---- | ----------- |
| `data[]` | `array` | List of roles. |
| `data[].id` | `integer` | Role identifier. |
| `data[].uuid` | `uuid` | Unique role identifier. |
| `data[].name` | `string` | Role machine name. |
| `data[].title` | `string` | Localized role title. |
| `data[].permissions[]` | `array` | Permission strings when `permissions=true`. |

---

## Notes

* Roles returned depend on the requesting platform's domain.
* The `permissions` parameter defaults to `false`.
