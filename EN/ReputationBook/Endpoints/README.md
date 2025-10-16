# ReputationBook API Endpoints

Complete list of all available endpoints in the ReputationBook domain organized by category.

## Public Complaint Endpoints

These endpoints are available for public access or with user authentication.

| Endpoint | Method | Route | Auth | Documentation |
|----------|--------|-------|------|---------------|
| List Statuses | GET | `/api/v1/complaints/statuses` | None | [ComplaintStatus.md](ComplaintStatus.md) |
| List Complaints | GET | `/api/v1/complaints` | None | [ComplaintIndex.md](ComplaintIndex.md) |
| Create Complaint | POST | `/api/v1/complaints` | Required | [ComplaintStore.md](ComplaintStore.md) |
| Show Complaint | GET | `/api/v1/complaints/{complaint}` | None | [ComplaintShow.md](ComplaintShow.md) |
| Update Complaint | PUT | `/api/v1/complaints/{complaint}` | Required | [ComplaintUpdate.md](ComplaintUpdate.md) |
| Update Status | PATCH | `/api/v1/complaints/{complaint}/status` | Required | [ComplaintUpdateStatus.md](ComplaintUpdateStatus.md) |
| Delete Complaint | DELETE | `/api/v1/complaints/{complaint}` | Required | [ComplaintDestroy.md](ComplaintDestroy.md) |
| Add Text | POST | `/api/v1/complaints/{complaint}/add-text` | Required | [ComplaintAddText.md](ComplaintAddText.md) |
| Add Reply | POST | `/api/v1/complaints/{complaint}/replies` | Required | [ComplaintReplyStore.md](ComplaintReplyStore.md) |

## User Complaint Book Endpoints

These endpoints allow users to manage their own complaints and profile.

| Endpoint | Method | Route | Auth | Documentation |
|----------|--------|-------|------|---------------|
| My Complaints | GET | `/api/v1/complaint-book/my-complaints` | Required | [ComplaintMy.md](ComplaintMy.md) |
| Show My Complaint | GET | `/api/v1/complaint-book/my-complaints/{complaint}` | Required | [ComplaintShowMy.md](ComplaintShowMy.md) |
| User Profile | GET | `/api/v1/complaint-book/me` | Required | [ComplaintBookMe.md](ComplaintBookMe.md) |
| Upload Image | POST | `/api/v1/complaint-book/me/image/upload` | Required | [ComplaintBookImageUpload.md](ComplaintBookImageUpload.md) |

## Backoffice Complaint Endpoints

These endpoints require `backoffice` ability with `domain:reputationbook` scope.

| Endpoint | Method | Route | Auth | Documentation |
|----------|--------|-------|------|---------------|
| Platform Info | GET | `/api/v1/complaint-book/me` | Backoffice | [BackofficeComplaintBookMe.md](BackofficeComplaintBookMe.md) |
| List Platform Complaints | GET | `/api/v1/reputation-book/complaints` | Backoffice | [BackofficeComplaintIndex.md](BackofficeComplaintIndex.md) |
| Create for User | POST | `/api/v1/reputation-book/complaints/{user}` | Backoffice | [BackofficeComplaintStore.md](BackofficeComplaintStore.md) |
| Show Platform Complaint | GET | `/api/v1/reputation-book/complaints/{complaint}` | Backoffice | [BackofficeComplaintShow.md](BackofficeComplaintShow.md) |
| Update Platform Complaint | PUT | `/api/v1/reputation-book/complaints/{complaint}` | Backoffice | [BackofficeComplaintUpdate.md](BackofficeComplaintUpdate.md) |
| Update Platform Status | PATCH | `/api/v1/reputation-book/complaints/{complaint}/status` | Backoffice | [BackofficeComplaintUpdateStatus.md](BackofficeComplaintUpdateStatus.md) |
| Delete Platform Complaint | DELETE | `/api/v1/reputation-book/complaints/{complaint}` | Backoffice | [BackofficeComplaintDestroy.md](BackofficeComplaintDestroy.md) |
| Store Image | POST | `/api/v1/reputation-book/complaints/{complaint}/images` | Backoffice | [BackofficeComplaintStoreImage.md](BackofficeComplaintStoreImage.md) |
| Delete Image | DELETE | `/api/v1/reputation-book/complaints/{complaint}/images/{uniqueId}` | Backoffice | [BackofficeComplaintDestroyImage.md](BackofficeComplaintDestroyImage.md) |
| Add Text | POST | `/api/v1/reputation-book/complaints/{complaint}/add-text` | Backoffice | [BackofficeComplaintAddText.md](BackofficeComplaintAddText.md) |

## Claimed Company Endpoints

These endpoints manage company claims and information.

| Endpoint | Method | Route | Auth | Documentation |
|----------|--------|-------|------|---------------|
| Show Company | GET | `/api/v1/reputation-book/claimed-company/{company}` | Optional | [ClaimedCompanyShow.md](ClaimedCompanyShow.md) |
| Create Company | POST | `/api/v1/reputation-book/claimed-company` | Backoffice | [ClaimedCompanyStore.md](ClaimedCompanyStore.md) |
| Update Company | PUT | `/api/v1/reputation-book/claimed-company/{company}` | Backoffice | [ClaimedCompanyUpdate.md](ClaimedCompanyUpdate.md) |
| Store Company Image | POST | `/api/v1/reputation-book/claimed-company/{company}/images` | Backoffice | [ClaimedCompanyStoreImage.md](ClaimedCompanyStoreImage.md) |
| Check RUC | GET | `/api/v1/reputation-book/claimed-company/ruc/{param}` | None | [ClaimedCompanyCheckRuc.md](ClaimedCompanyCheckRuc.md) |

## Analytics Endpoints

These endpoints provide statistics and reports.

| Endpoint | Method | Route | Auth | Documentation |
|----------|--------|-------|------|---------------|
| Complaint Counters | GET | `/api/v1/reputation-book/complaints/counter` | Backoffice | [ComplaintCounterIndex.md](ComplaintCounterIndex.md) |
| Complaint Report | GET | `/api/v1/reputation-book/complaints/report/{interval}` | Backoffice | [ComplaintCounterReport.md](ComplaintCounterReport.md) |

## Collaborator Endpoints

These endpoints manage collaborator profiles.

| Endpoint | Method | Route | Auth | Documentation |
|----------|--------|-------|------|---------------|
| Collaborator Profile | GET | `/api/v1/reputation-book/collaborators/me` | Backoffice | [CollaboratorMe.md](CollaboratorMe.md) |

## Authentication Types

- **None**: No authentication required (public endpoints)
- **Required**: Bearer token required (`Authorization: Bearer {token}`)
- **Backoffice**: Bearer token with `backoffice` ability and `domain:reputationbook` scope required
- **Optional**: Authentication enhances the response but is not required

## Common Patterns

### Pagination

Most list endpoints support pagination with these parameters:
- `per_page` - Items per page (default: 15)
- `page` - Page number
- `no_paginate` - Disable pagination (use with `limit`)

### Filtering

List endpoints typically support:
- Status filtering
- Date range filtering
- Company filtering
- User filtering
- Text search

### Localization

Endpoints respect the `Accept-Language` header:
- `en` - English
- `es` - Spanish (Español)
- `pt-BR` - Portuguese (Português)

## Related Documentation

- [Domain Overview](../README.md) - ReputationBook domain introduction
- [Shared Endpoints](../../Shared/Endpoints/README.md) - Cross-domain utilities

## Total Endpoints

- Public: 9 endpoints
- User: 4 endpoints
- Backoffice: 10 endpoints
- Claimed Company: 5 endpoints
- Analytics: 2 endpoints
- Collaborator: 1 endpoint

**Total: 31 endpoint routes (28 unique documentation files)**
