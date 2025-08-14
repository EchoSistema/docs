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
| `no_paginate` | `boolean` | No | Disable pagination when `true`. |
| `per_page` | `integer` | No | Items per page when paginated. |
| `sort_by` | `string` | No | Column to sort by. |
| `order_by` | `string` | No | Sort direction (`ASC` or `DESC`). |
| `with_count` | `array` | No | Relationships for count aggregation. |
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
      "uuid": "5ca829a0-aab2-3991-a8de-817d33b7dbcf",
      "name": "1 Empresa XPTO",
      "assumed_name": null,
      "slug": "1-empresa-xpto",
      "created_at": "2025-06-30T05:11:58-03:00",
      "address": "General Cabañas, 233, Juan León Mallorquín, Encarnación, Itapuá, PY",
      "fiscal_documents": [
        {
          "id": 1002,
          "type": "ruc",
          "value": "123561"
        }
      ]
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
| `data[].address` | `string` | Company address. |
| `data[].fiscal_documents[]` | `array` | List of fiscal documents. |
| `data[].fiscal_documents[].id` | `integer` | Fiscal document ID. |
| `data[].fiscal_documents[].type` | `string` | Type of fiscal document. |
| `data[].fiscal_documents[].value` | `string` | Value of fiscal document. |

---

## Notes

* Pagination metadata is omitted when `no_paginate=true`.
* String filters are case-insensitive partial matches unless otherwise noted.

