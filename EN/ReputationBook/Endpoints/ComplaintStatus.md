# ReputationBook – List Complaint Statuses

## Endpoint

```
GET /api/v1/complaints/statuses
```

## Authentication

None

## Headers

| Header           | Type   | Required | Description |
| ---------------- | ------ | -------- | ----------- |
| X-PUBLIC-KEY     | string | Yes      | Platform public key. |
| Accept-Language  | string | No       | IETF locale (e.g., `pt-BR`, `en`, `es`). |

## Parameters

No parameters required.

## Examples

### Request example (curl)

```bash
curl -X GET \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: en" \
  "https://sandbox.your-domain.com/api/v1/complaints/statuses"
```

### Response example

```json
{
  "data": [
    {
      "value": "pending",
      "name": "PENDING",
      "title": "Pending"
    },
    {
      "value": "in_progress",
      "name": "IN_PROGRESS",
      "title": "In Progress"
    },
    {
      "value": "resolved",
      "name": "RESOLVED",
      "title": "Resolved"
    },
    {
      "value": "rejected",
      "name": "REJECTED",
      "title": "Rejected"
    }
  ]
}
```

## JSON Structure Explained

| Field       | Type    | Description |
| ----------- | ------- | ----------- |
| data[]      | array   | List of available complaint statuses. |
| data[].value | string | The status value used in API requests. |
| data[].name  | string | The internal constant name of the status. |
| data[].title | string | Localized display title for the status. |

## HTTP Status

- 200: OK

## Errors

No specific error cases for this endpoint.

## Notes

- This endpoint returns all available complaint status options.
- The `title` field is localized based on the `Accept-Language` header.
- No authentication is required to retrieve the list of statuses.

## Related

- [Complaint Index](ComplaintIndex.md)
- [Complaint Update Status](ComplaintUpdateStatus.md)
