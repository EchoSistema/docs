# ReputationBook – List Complaint Statuses

## Endpoint

```
GET /api/v1/complaints/statuses
```

Returns every possible value from `ComplaintStatusEnum`, including the technical value, enum name, and localized title.

## Authentication

None. Only the `X-PUBLIC-KEY` header is required by the `platform` middleware.

## Headers

| Header | Type | Required | Description |
| ------ | ---- | -------- | ----------- |
| `X-PUBLIC-KEY` | string | Yes | Identifies the requesting platform. |
| `Accept-Language` | string | Optional | Helps the frontend pick the desired locale; the endpoint already returns `title()` translated. |

## Example Response (200)

```json
{
  "data": [
    {
      "value": "open",
      "name": "OPEN",
      "title": "Open"
    },
    {
      "value": "in_progress",
      "name": "IN_PROGRESS",
      "title": "In progress"
    },
    {
      "value": "resolved",
      "name": "RESOLVED",
      "title": "Resolved"
    }
  ]
}
```

## JSON Structure

| Field | Type | Description |
| ----- | ---- | ----------- |
| `data[]` | array | List of all available statuses. |
| `data[].value` | string | Persisted enum value. |
| `data[].name` | string | Uppercase enum constant name. |
| `data[].title` | string | Localized label meant for presentation. |

## HTTP Status Codes

- 200: List returned.
- 500: Unexpected error.

