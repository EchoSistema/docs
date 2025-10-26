# ReputationBook – View Complaint Details (Backoffice)

## Endpoint

```
GET /api/v1/backoffice/complaints/{complaint}
```

## Description

Returns detailed information about a specific complaint, including full texts, images, replies, status change history, and all related data. Exclusive endpoint for backoffice administrators.

## Authentication

Required – Bearer {token} with `backoffice` ability

## Headers

| Header           | Type   | Required | Description |
| ---------------- | ------ | -------- | ----------- |
| Authorization    | string | Yes      | `Bearer {token}` credential. |
| X-PUBLIC-KEY     | string | Yes      | Platform public key. |
| Accept-Language  | string | No       | IETF locale (e.g., `pt-BR`, `en`, `es`). |

## URL Parameters

| Parameter  | Type   | Required | Description |
| ---------- | ------ | -------- | ----------- |
| complaint  | string | Yes      | UUID of the complaint to view |

## Example Response

```json
{
  "uuid": "9d4e1c2a-3b4c-5d6e-7f8g-9h0i1j2k3l4m",
  "is_public": true,
  "status": "replied",
  "purchase_date": "2024-01-10T00:00:00Z",
  "invoice_number": "INV-2024-001",
  "created_at": "2024-01-15T10:30:00Z",
  "user": {
    "uuid": "8c3d0b1a-2e3f-4g5h-6i7j-8k9l0m1n2o3p",
    "name": "John Doe",
    "email": "john.doe@example.com",
    "gender": "Male"
  },
  "texts": [
    {
      "content": "<p>I bought the product on 01/10/2024 and noticed it was defective...</p>",
      "usage": "complaint_text",
      "language": "en",
      "is_default": true,
      "created_at": "2024-01-15T10:30:00Z"
    }
  ],
  "images": [
    {
      "uuid": "4a9b6c7d-8e9f-0g1h-2i3j-4k5l6m7n8o9p",
      "usage": "evidence",
      "url": "https://cdn.example.com/complaints/evidence1.jpg",
      "created_at": "2024-01-15T10:40:00Z"
    }
  ],
  "replies": [
    {
      "uuid": "2a7b4c5d-6e7f-8g9h-0i1j-2k3l4m5n6o7p",
      "is_support": false,
      "index_number": 1,
      "created_at": "2024-01-16T14:20:00Z",
      "user": {
        "uuid": "1a6b3c4d-5e6f-7g8h-9i0j-1k2l3m4n5o6p",
        "name": "Customer Service",
        "email": "support@company.com"
      },
      "text": {
        "content": "Hello, we apologize. We'll arrange a product replacement.",
        "language": "en",
        "is_default": true,
        "usage": "reply_text"
      }
    }
  ],
  "status_change_history": [
    {
      "previous_status": null,
      "status": "initiated",
      "updated_by": null,
      "changed_at": "2024-01-15T10:30:00Z"
    },
    {
      "previous_status": "initiated",
      "status": "pending",
      "updated_by": "Admin John",
      "changed_at": "2024-01-15T11:00:00Z"
    }
  ]
}
```

## Loaded Relations

This endpoint automatically loads the following relations per `Complaint::RESOURCE_RELATIONS`:

- `user` - User who created the complaint
- `platform` - Platform where registered
- `company` - Complained company with full details
- `company.addresses` - Company addresses
- `company.contacts` - Company contacts
- `company.socialMedias` - Social media
- `company.rating` - Company rating
- `titles` - Multilingual titles
- `description` - Detailed description
- `images` - Attached images
- `replies` - Complaint replies
- `statusChangeEvents` - Change history

## HTTP Status

- 200: OK
- 401: Unauthorized
- 403: Forbidden (no `backoffice` ability)
- 404: Complaint not found
- 422: Validation error
- 429: Too many requests
- 500: Internal error

## Conditional Fields

The following field blocks are conditional:

- **user**: When user is loaded
- **platform**: When platform is loaded
- **company**: When associated with a company
- **texts**: When texts are loaded
- **images**: When images exist
- **replies**: When there are replies
- **status_change_history**: When status history exists

## Notes

- Returns much more detailed information than index
- All sensitive data is exposed (administrative endpoint)
- User emails are always visible
- Status history shows complete complaint evolution
- Replies are sorted by `index_number`
- `updated_by` in history is `null` when changed by complaint owner
- Texts and descriptions may contain HTML
- All image URLs are complete and ready to use
- Dates follow ISO 8601 format

## Related

- [List Complaints](BackofficeComplaintIndex.md)

## Changelog

- 2025-10-26: Initial backoffice endpoint documentation
