# Company – Index Companies Through Coverage Endpoint

## Endpoint

`GET /api/v1/company/through-coverage`

Retrieves companies available to the current platform's coverage. Supports filtering, sorting, and pagination.

---

## Authentication

None.

---

## Request

### Required Headers

| Header | Type | Description |
| ------ | ---- | ----------- |
| **X-PUBLIC-KEY** | `string` | Platform public key required to authenticate and receive responses. |
| **Accept-Language** | `string` | Locale for translatable fields (e.g., `en`, `pt-BR`). Optional. |

### Filter Parameters

| Parameter | Type | Required | Description |
| --------- | ---- | -------- | ----------- |
| `id` | `integer` | No | Company ID. |
| `uuids` | `array[uuid]` | No | Filter by specific company UUIDs. |
| `name` | `string` | No | Company name. |
| `slug` | `string` | No | Company slug. |
| `fiscal_document_type` | `FiscalDocumentTypeEnum` | No | Type of fiscal document. |
| `fiscal_document_value` | `string` | No | Value of fiscal document. |
| `relations` | `array` | No | Include extra relations: `logos`, `contacts`, `fiscalDocuments`, `address`. |
| `no_paginate` | `boolean` | No | Disable pagination. Accepts `true`, `false`, `0`, or `1`. |
| `per_page` | `integer` | No | Items per page when paginated. |
| `page` | `integer` | No | Page number when paginated. |
| `sort_by` | `string` | No | Column to sort by. |
| `order_by` | `string` | No | Sort direction (`ASC` or `DESC`). |
| `with_count` | `array` | No | Relationships to count (e.g., `complaints`, `reviews`). |
| `with_exists` | `array` | No | Relationships to check existence (e.g., `complaints`, `reviews`). |
| `no_complaints` | `boolean` | No | Companies without complaints. |
| `has_complaints` | `boolean` | No | Companies with complaints. |
| `no_reviews` | `boolean` | No | Companies without reviews. |
| `has_reviews` | `boolean` | No | Companies with reviews. |
| `rating` | `float` | No | Average rating filter. |
| `good_rating` | `boolean` | No | Filter companies with good ratings. |
| `bad_rating` | `boolean` | No | Filter companies with poor ratings. |

> Parameters accept camelCase, snake_case, kebab-case, or CapitalCase variants.

---

## Example Response

```json
{
  "data": [
    {
      "created_by": null,
      "uuid": "00000000-0000-0000-0000-000000000000",
      "name": "Example Company",
      "assumed_name": "Example Trade Name",
      "slug": "example-company",
      "created_at": "2025-01-01T00:00:00-03:00",
      "address": "123 Example St, Sample City, EX",
      "fiscal_documents": [
        {
          "id": 1,
          "type": "ruc",
          "value": "00000001"
        }
      ],
      "logos": [
        {
          "unique_id": "123456",
          "usage": "logo",
          "url": "https://example.com/logo.svg",
          "name": "logo.svg",
          "slug": "logo",
          "width": 250,
          "height": 250,
          "creation_date": "2025-01-01 00:00:00"
        }
      ],
      "contacts": [
        {
          "id": 1,
          "type": "email",
          "email": "contact@example.com"
        },
        {
          "id": 2,
          "type": "telephone",
          "phone": "+1 234 567 8900",
          "country_code": "+1",
          "number": "234 567 8900"
        }
      ],
      "complaints_count": 0,
      "reviews_count": 0,
      "complaints_exists": false,
      "reviews_exists": false
    }
  ]
}
```

---

## JSON Structure Explanation

| Field | Type | Description |
| ----- | ---- | ----------- |
| `data[]` | `array` | List of company summaries. |
| `data[].uuid` | `uuid` | Company identifier. |
| `data[].name` | `string` | Company name. |
| `data[].slug` | `string` | URL-friendly slug. |
| `data[].assumed_name` | `string` | Trade name, if any. |
| `data[].created_at` | `datetime` | Creation timestamp. |
| `data[].address` | `string` | Company address when `relations` includes `address`. |
| `data[].fiscal_documents[]` | `array` | Fiscal documents when `relations` includes `fiscalDocuments`. |
| `data[].fiscal_documents[].id` | `integer` | Fiscal document ID. |
| `data[].fiscal_documents[].type` | `string` | Type of fiscal document. |
| `data[].fiscal_documents[].value` | `string` | Value of fiscal document. |
| `data[].logos[]` | `array` | Company logos when `relations` includes `logos`. |
| `data[].contacts[]` | `array` | Company contacts when `relations` includes `contacts`. |
| `data[].complaints_count` | `integer` | Complaints count when `with_count` includes `complaints`. |
| `data[].reviews_count` | `integer` | Reviews count when `with_count` includes `reviews`. |
| `data[].complaints_exists` | `boolean` | Indicates if complaints exist when `with_exists` includes `complaints`. |
| `data[].reviews_exists` | `boolean` | Indicates if reviews exist when `with_exists` includes `reviews`. |

---

## Notes

* Pagination metadata is omitted when `no_paginate` is truthy.
* Additional relations, counts, and existence flags appear only when requested.
* String filters are case-insensitive partial matches unless otherwise noted.

