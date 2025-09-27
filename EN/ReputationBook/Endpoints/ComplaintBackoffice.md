# ReputationBook – Complaint Management (Backoffice)

All routes below require `Bearer {token}` authentication (`auth:sanctum`) with abilities `backoffice` and `domain:reputationbook`.

## List Platform Complaints

```
GET /api/v1/complaint-book/complaints
```

- Accepts the filters from `ComplaintFilterRequest`.
- Returns `ComplaintCollection` including users, companies, and texts.
- Supports `no_paginate=true` to retrieve a limited list (`limit`).

## Create Complaint on Behalf of a User

```
POST /api/v1/complaint-book/complaints/{user}
```

| Parameter | Type | Description |
| --------- | ---- | ----------- |
| `user` | string | ID or UUID of the user being represented. |

### Request Body

Same payload as `POST /api/v1/complaints`, following `ComplaintStoreRequest`. The field `created_by` is automatically filled with the authenticated operator UUID.

### Response

`ComplaintResource` with basic relations (`images`, `company`, `platform`).

## Show Details

```
GET /api/v1/complaint-book/complaints/{complaint}
```

Returns the complaint with extended relations (`images`, `statusChangeEvents`, `company` full resource).

## Update Complaint Data

```
PUT /api/v1/complaint-book/complaints/{complaint}
```

Uses the same fields as `ComplaintUpdateRequest`.

## Update Complaint Status

```
PATCH /api/v1/complaint-book/complaints/{complaint}/status
```

| Field | Type | Required |
| ----- | ---- | -------- |
| `status` | string | Yes (`ComplaintStatusEnum`). |

The authenticated operator is stored in the timeline (`statusChangeEvents`).

## Add Complementary Text

```
POST /api/v1/complaint-book/complaints/{complaint}/add-text
```

Follows `TextStoreRequest` (`content`, `language`, `usage`).

## Upload Evidence Image

```
POST /api/v1/complaint-book/complaints/{complaint}/images
```

Uses `ImageStoreRequest`. Provide `usage` (e.g., `evidence`) and supply the image through URL, Base64, or file upload.

## Remove a Specific Image

```
DELETE /api/v1/complaint-book/complaints/{complaint}/images/{uniqueId}
```

- Requires the `destroy` / `delete` permission.
- Deletes the image whose hexadecimal identifier (`hex`) matches `uniqueId`.
- Returns the updated `ComplaintResource`.

## Delete Complaint

```
DELETE /api/v1/complaint-book/complaints/{complaint}
```

Removes the complaint and returns `204 No Content`.

## HTTP Status Codes

- 200: Operation completed with payload (`GET`, `PUT`, `PATCH`, `POST add-text/images`).
- 201: Images created.
- 204: Complaint removed.
- 400/422: Validation failed.
- 401/403: Missing authentication or abilities.
- 404: Complaint or image not found.

