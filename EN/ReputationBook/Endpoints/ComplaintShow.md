# ReputationBook – Show Complaint Details

## Endpoint

```
GET /api/v1/complaints/{complaint}
```

Retrieves detailed information about a specific public complaint.

## Authentication

None (public endpoint)

## Headers

| Header           | Type   | Required | Description |
| ---------------- | ------ | -------- | ----------- |
| X-PUBLIC-KEY     | string | Yes      | Platform public key. |
| Accept-Language  | string | No       | IETF locale (e.g., `pt-BR`, `en`, `es`). |

## Parameters

### Path parameters

| Parameter | Type   | Required | Description |
| --------- | ------ | -------- | ----------- |
| complaint | uuid   | Yes      | Complaint UUID or ID. |

## Examples

### Request example (curl)

```bash
curl -X GET \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: en" \
  "https://sandbox.your-domain.com/api/v1/complaints/9d1234ab-5678-90ef-1234-567890abcdef"
```

### Response example

```json
{
  "data": {
    "uuid": "9d1234ab-5678-90ef-1234-567890abcdef",
    "user": {
      "uuid": "9a1234ab-5678-90ef-1234-567890abcdef",
      "name": "John Doe",
      "gender": "male"
    },
    "platform": {
      "uuid": "9b1234ab-5678-90ef-1234-567890abcdef",
      "name": "Platform Name"
    },
    "company": {
      "uuid": "9e1234ab-5678-90ef-1234-567890abcdef",
      "name": "Company ABC",
      "slug": "company-abc"
    },
    "titles": [
      {
        "content": "Product quality issue",
        "language": "en",
        "is_default": true
      }
    ],
    "texts": [
      {
        "content": "The product received was defective.",
        "language": "en",
        "is_default": true
      }
    ],
    "images": [
      {
        "uuid": "9f1234ab-5678-90ef-1234-567890abcdef",
        "url": "https://cdn.example.com/images/complaint-image.jpg",
        "usage": "complaint_evidence"
      }
    ],
    "status": "pending",
    "is_public": true,
    "purchase_date": "2025-10-01",
    "created_at": "2025-10-15T14:30:00.000000Z",
    "updated_at": "2025-10-15T14:30:00.000000Z"
  }
}
```

## JSON Structure Explained

| Field       | Type    | Description |
| ----------- | ------- | ----------- |
| data        | object  | The complaint object. |
| data.uuid   | uuid    | Unique identifier for the complaint. |
| data.user   | object  | User who created the complaint. |
| data.platform | object | Platform where the complaint was registered. |
| data.company | object | Company being complained about. |
| data.titles[] | array | Localized complaint titles. |
| data.texts[] | array  | Localized complaint text content. |
| data.images[] | array | Attached images. |
| data.status | string  | Current complaint status. |
| data.is_public | boolean | Whether the complaint is publicly visible. |
| data.purchase_date | date | Date of purchase related to the complaint. |
| data.created_at | string | Complaint creation timestamp (ISO-8601). |
| data.updated_at | string | Complaint last update timestamp (ISO-8601). |

## HTTP Status

- 200: OK
- 404: Not Found
- 500: Internal Server Error

## Errors

```json
{
  "message": "Resource not found"
}
```

## Notes

- This endpoint returns public complaints only.
- The `titles` and `texts` arrays are filtered by the `Accept-Language` header.
- If no localized version exists, the default language version is returned.

## Related

- [Complaint Index](ComplaintIndex.md)
- [Complaint Store](ComplaintStore.md)
- [Complaint Update](ComplaintUpdate.md)
