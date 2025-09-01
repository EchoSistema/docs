# Platform – List Platform Users Endpoint Documentation

## Endpoint

`GET /api/v1/reputation-book/users`
`GET /api/v1/ia/admin/users`

Retrieves a list of platform users with role, occupation and area-based filtering. Supports fine-grained search criteria and internationalisation.

---

## Authentication

**Required** – Bearer token with `backoffice` ability.

---

## Request

### Required Headers

| Header              | Type     | Description |
| ------------------- | -------- | ----------- |
| **Authorization**   | `string` | `Bearer {token}` credential. **Required**. |
| **X-PUBLIC-KEY**    | `string` | Public key that identifies the requesting platform. **Required**. |
| **Accept-Language** | `string` | IETF language tag (e.g. `en`, `es`, `pt-BR`). Determines localisation for translatable fields. **Required**. |

### Filter Parameters

| Parameter                 | Type           | Required | Description |
| ------------------------- | -------------- | -------- | ----------- |
| `role`                    | `string/int`   | No       | Role identifier (numeric ID or name). |
| `roles[]`                 | `array`        | No       | List of roles (numeric IDs or names). |
| `role_id`                 | `integer`      | No       | Role numeric ID. |
| `role_name`               | `string`       | No       | Role name. |
| `role_ids[]`              | `array`        | No       | List of role numeric IDs. |
| `role_names[]`            | `array`        | No       | List of role names. |
| `name`                    | `string`       | No       | User's name (partial or full match). |
| `email`                   | `string`       | No       | User's email (exact match, case-insensitive). |
| `user_name`               | `string`       | No       | Alias for `name`. |
| `user_email`              | `string`       | No       | Alias for `email`. |
| `user_uuid`               | `uuid`         | No       | User UUID. |
| `job_occupation`          | `string/int`   | No       | Job occupation ID (numeric), UUID, or title. |
| `job_occupation_id`       | `string`       | No       | Job occupation numeric ID. |
| `job_occupation_uuid`     | `string`       | No       | Job occupation UUID. |
| `job_occupation_title`    | `string`       | No       | Job occupation title. |
| `occupation_area`         | `string/array` | No       | Occupation area as UUID, as `content:usage` string or structured array with `content` and `usage`. |
| `occupation_area.content` | `string`       | No       | Occupation area name or keyword. |
| `occupation_area.usage`   | `string`       | No       | Usage type for occupation area. Allowed: `occupation_area_title`. |
| `occupation_area_id`      | `string`       | No       | Occupation area numeric ID. |
| `occupation_area_uuid`    | `string`       | No       | Occupation area UUID. |
| `has_job_occupation`      | `boolean`      | No       | Filter users by presence (`true`) or absence (`false`) of a job occupation. |
| `has_occupation_area`     | `boolean`      | No       | Filter users by presence (`true`) or absence (`false`) of an occupation area. |
| `no_paginate`             | `boolean`      | No       | When `true`, disables pagination and returns the full result set. Defaults to `false`. |
| `per_page`                | `integer`      | No       | Items per page for pagination. Defaults to `25`. |

> **Note:** All filter parameters also accept camelCase variants (e.g. `roleId`, `userEmail`).

---

## Example Response Object

```json
{
  "uuid": "9b732d6a-2e56-4db8-9b2e-6fd8c14e7f28",
  "name": "John Doe",
  "email": "john.doe@example.com",
  "image": "https://cdn.example.com/avatars/johndoe.webp",
  "gender": {
    "abbr": "M",
    "name": "Male"
  },
  "birth_date": "1990-05-15T00:00:00+00:00",
  "age": 34,
  "language": "en",
  "currency": {
    "id": "USD",
    "name": "US Dollar",
    "sign": "$"
  },
  "role": {
    "id": 2,
    "name": "Admin",
    "localized_name": "Administrator",
    "created_at": "2024-05-01T12:00:00+00:00"
  },
  "telephone": "+1-202-555-0147",
  "addresses": [
    "123 Example Street, District Name, Sample City, Sample State, Sample Country"
  ],
  "platform": {
    "user_status": "active",
    "name": "PlatformName"
  },
  "occupation": {
    "uuid": "bb4a7f72-39b6-4d23-a5c5-7186f1d2bcb3",
    "title": "Software Engineer",
    "is_default": true
  },
  "created_at": "2024-05-01T12:00:00+00:00",
  "updated_at": "2024-08-20T09:30:00+00:00"
}
```

---

## JSON Structure Explanation

### `data[]` – User Object

| Field         | Type     | Description |
| ------------- | -------- | ----------- |
| `uuid`        | `uuid`   | Unique identifier for the user. |
| `name`        | `string` | User's full name. |
| `email`       | `string` | User's email address. |
| `image`       | `string` | URL to the user's avatar image (if available). |
| `gender`      | `object` | Gender information with abbreviation and full name. |
| `birth_date`  | `string` | Birth date in ISO-8601 format. |
| `age`         | `int`    | Calculated age of the user. |
| `language`    | `string` | User's preferred language. |
| `currency`    | `object` | Currency information with `id` (ISO code), `name`, and `sign`. |
| `role`        | `object` | Role assigned to the user on the platform, with ID, name, localized name, and creation date. |
| `telephone`   | `string` | User's telephone number, if available. |
| `addresses[]` | `array`  | List of formatted addresses linked to the user. |
| `platform`    | `object` | Platform-specific user data, including `user_status` and `name` (only when `platform` query provided). |
| `occupation`  | `object` | Occupation information if the user has job occupations loaded. |
| `created_at`  | `string` | Date when the role assignment was created (ISO-8601 format). |
| `updated_at`  | `string` | Date when the user's data was last updated (ISO-8601 format). |

### `gender`

| Field  | Type     | Description                     |
| ------ | -------- | ------------------------------- |
| `abbr` | `string` | Gender abbreviation (e.g. `M`). |
| `name` | `string` | Full gender name.               |

### `currency`

| Field  | Type     | Description           |
| ------ | -------- | --------------------- |
| `id`   | `string` | Currency ISO code.    |
| `name` | `string` | Full currency name.   |
| `sign` | `string` | Currency symbol/sign. |

### `role`

| Field            | Type     | Description                                           |
| ---------------- | -------- | ----------------------------------------------------- |
| `id`             | `int`    | Role numeric ID.                                      |
| `name`           | `string` | Internal role name.                                   |
| `localized_name` | `string` | Localized display name for the role.                  |
| `created_at`     | `string` | Date role was assigned to the user (ISO-8601 format). |

### `occupation`

| Field        | Type   | Description                                         |
| ------------ | ------ | --------------------------------------------------- |
| `uuid`       | `uuid` | Job experience UUID.                                |
| `title`      | `string` | Job title.                                         |
| `is_default` | `bool` | Indicates if this is the user's default occupation. |

---

## Notes

* Users can only list users with roles lower than their own.
* If the query includes `no_paginate=true`, pagination metadata is omitted and all results are returned.
* String filters are **case-insensitive partial matches** unless otherwise noted.
* The parameters `role`, `roles`, `name`, and `email` are mapped internally to their `role_id`, `role_ids`, `user_name`, and `user_email` equivalents before filtering.
* Occupation and occupation area filters automatically set the `has_job_occupation` and `has_occupation_area` flags.
* All filter parameters accept camelCase equivalents.
