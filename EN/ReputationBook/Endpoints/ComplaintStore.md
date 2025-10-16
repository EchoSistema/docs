# ReputationBook – Create Complaint

## Endpoint

```
POST /api/v1/complaints
```

Creates a new complaint against a company.

## Authentication

**Required** – Bearer token with `backoffice` ability and `domain:reputationbook` scope.

## Headers

| Header           | Type   | Required | Description |
| ---------------- | ------ | -------- | ----------- |
| Authorization    | string | Yes      | `Bearer {token}` credential. |
| X-PUBLIC-KEY     | string | Yes      | Platform public key. |
| Accept-Language  | string | No       | IETF locale (e.g., `pt-BR`, `en`, `es`). |
| Content-Type     | string | Yes      | Must be `application/json`. |

## Parameters

### Request body

```json
{
  "company_uuid": "9e1234ab-5678-90ef-1234-567890abcdef",
  "purchase_date": "2025-10-01",
  "title": "Product quality issue",
  "complaint": "The product received was defective and did not match the description.",
  "is_public": true,
  "invoice_number": "INV-2025-001",
  "attendants_name": "Jane Doe",
  "language": "en"
}
```

| Parameter | Type | Required | Description |
| --------- | ---- | -------- | ----------- |
| company_uuid | uuid | Yes* | Company UUID. Required if `company_id` is not provided. |
| company_id | integer | Yes* | Company ID. Required if `company_uuid` is not provided. |
| purchase_date | date | Yes | Date of purchase (format: YYYY-MM-DD). Must be today or earlier. |
| title | string | Yes | Complaint title. |
| complaint | string | Yes | Detailed complaint description. |
| is_public | boolean | No | Whether the complaint should be publicly visible. Defaults to `false`. |
| invoice_number | string | No | Invoice or receipt number. |
| attendants_name | string | No | Name of the staff member who attended. |
| language | string | No | Language code for the complaint (e.g., `en`, `es`, `pt-BR`). |

> Either `company_uuid` or `company_id` must be provided, but not both.

## Examples

### Request example (curl)

```bash
curl -X POST \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Content-Type: application/json" \
  -H "Accept-Language: en" \
  -d '{
    "company_uuid": "9e1234ab-5678-90ef-1234-567890abcdef",
    "purchase_date": "2025-10-01",
    "title": "Product quality issue",
    "complaint": "The product received was defective.",
    "is_public": true
  }' \
  "https://sandbox.your-domain.com/api/v1/complaints"
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
    "images": [],
    "status": "pending",
    "is_public": true,
    "purchase_date": "2025-10-01",
    "invoice_number": "INV-2025-001",
    "attendants_name": "Jane Doe",
    "created_at": "2025-10-15T14:30:00.000000Z",
    "updated_at": "2025-10-15T14:30:00.000000Z"
  }
}
```

## JSON Structure Explained

| Field       | Type    | Description |
| ----------- | ------- | ----------- |
| data        | object  | The created complaint object. |
| data.uuid   | uuid    | Unique identifier for the complaint. |
| data.user   | object  | User who created the complaint. |
| data.company | object | Company being complained about. |
| data.titles[] | array | Localized complaint titles. |
| data.texts[] | array  | Localized complaint text content. |
| data.images[] | array | Attached images (empty on creation). |
| data.status | string  | Initial status (always `pending`). |
| data.is_public | boolean | Public visibility setting. |
| data.purchase_date | date | Date of purchase. |
| data.invoice_number | string | Invoice number (if provided). |
| data.attendants_name | string | Attendant name (if provided). |
| data.created_at | string | Creation timestamp (ISO-8601). |
| data.updated_at | string | Last update timestamp (ISO-8601). |

## HTTP Status

- 201: Created
- 400: Bad Request
- 401: Unauthorized
- 403: Forbidden
- 404: Not Found (company not found)
- 422: Unprocessable Entity (validation errors)
- 429: Too Many Requests
- 500: Internal Server Error

## Errors

```json
{
  "message": "Validation error",
  "errors": {
    "company_uuid": ["The company uuid field is required when company id is not present."],
    "purchase_date": ["The purchase date field is required."],
    "title": ["The title field is required."],
    "complaint": ["The complaint field is required."]
  }
}
```

## Notes

- Requires authentication with `backoffice` ability and `domain:reputationbook` scope.
- The complaint is created with `pending` status by default.
- User authorization is checked before creating the complaint.
- The `language` field determines the language of the title and text content.
- Images can be added separately after complaint creation.

## Related

- [Complaint Index](ComplaintIndex.md)
- [Complaint Show](ComplaintShow.md)
- [Complaint Update](ComplaintUpdate.md)
- [Complaint Add Text](ComplaintAddText.md)
