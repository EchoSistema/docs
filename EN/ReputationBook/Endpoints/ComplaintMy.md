# ReputationBook – Authenticated User Complaints

## List My Complaints

### Endpoint

```
GET /api/v1/complaint-book/my-complaints
```

Returns the complaints created by the authenticated user for the current platform.

### Authentication

Required – `Bearer {token}` via `auth:sanctum`.

### Headers

| Header | Type | Required | Description |
| ------ | ---- | -------- | ----------- |
| `Authorization` | string | Yes | Valid bearer token. |
| `X-PUBLIC-KEY` | string | Yes | Current platform key. |
| `Accept-Language` | string | Recommended | Locale used to fetch titles/texts. |

### Query Parameters

Supports the same filters as `ComplaintFilterRequest`, but the API forces `user_id = Auth::id()` internally.

| Parameter | Type | Description |
| --------- | ---- | ----------- |
| `page`, `per_page`, `no_paginate`, `limit` | integer / boolean | Pagination controls. |
| `status` | string | Filter by status (`open`, `resolved`, ...). |
| `created_at`, `created_at_period[]` | date / array | Creation date filters. |
| `purchase_date`, `purchase_date_period[]` | date / array | Purchase date filters. |
| `with_texts` | boolean | Includes full texts in the payload. |

### Response (200)

Shares the same structure as the public listing, but includes additional fields (`texts`, `images`, `user`) from `ComplaintResource`.

### HTTP Status Codes

- 200: List returned.
- 401: Missing or expired token.
- 500: Internal error.

---

## Show a Complaint I Own

### Endpoint

```
GET /api/v1/complaint-book/my-complaints/{complaint}
```

Returns complete details for a complaint owned by the authenticated user. Accepts ID, UUID, or slug.

### Authentication

Required – `Bearer {token}` (`auth:sanctum`).

### Path Parameters

| Parameter | Type | Description |
| --------- | ---- | ----------- |
| `complaint` | string | Complaint identifier (ID/UUID/slug). |

### HTTP Status Codes

- 200: Complaint found.
- 403: Complaint belongs to another user.
- 404: Complaint not found.

### Notes

- The response includes platform, company, full texts, images, and status events when loaded.

