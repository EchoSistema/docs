# ReputationBook Domain API Documentation

The ReputationBook domain provides a comprehensive complaint management system that allows users to file, track, and manage complaints against companies. It includes features for claim management, company information, and administrative tools for platform operators.

## Overview

The ReputationBook (Libro de Quejas / Livro de Reclamações) API enables:

- **Complaint Management**: Create, read, update, and delete complaints
- **Status Tracking**: Monitor complaint lifecycle and resolution status
- **Public & Private Complaints**: Control complaint visibility
- **Company Claims**: Register and manage claimed companies
- **Reply System**: Enable communication between users, companies, and platform support
- **Administrative Tools**: Backoffice endpoints for platform operators
- **Analytics**: Complaint counters and reporting

## Authentication

Most endpoints in this domain require authentication with specific abilities:

- **Public Endpoints**: No authentication required (e.g., viewing public complaints)
- **User Endpoints**: Require `Bearer {token}` (e.g., managing own complaints)
- **Backoffice Endpoints**: Require `Bearer {token}` with `backoffice` ability and `domain:reputationbook` scope

## Base Endpoints

### Public Complaint Endpoints

- `GET /api/v1/complaints/statuses` - List available complaint statuses
- `GET /api/v1/complaints` - List public complaints
- `GET /api/v1/complaints/{complaint}` - Show complaint details
- `POST /api/v1/complaints` - Create new complaint (authenticated)
- `PUT /api/v1/complaints/{complaint}` - Update complaint (authenticated)
- `PATCH /api/v1/complaints/{complaint}/status` - Update complaint status (authenticated)
- `DELETE /api/v1/complaints/{complaint}` - Delete complaint (authenticated)
- `POST /api/v1/complaints/{complaint}/add-text` - Add text to complaint (authenticated)
- `POST /api/v1/complaints/{complaint}/replies` - Add reply to complaint (authenticated)

### User Complaint Book Endpoints

- `GET /api/v1/complaint-book/my-complaints` - List user's own complaints
- `GET /api/v1/complaint-book/my-complaints/{complaint}` - Show user's complaint
- `GET /api/v1/complaint-book/me` - Get user profile
- `POST /api/v1/complaint-book/me/image/upload` - Upload user profile image

### Backoffice Endpoints

- `GET /api/v1/complaint-book/me` - Get platform company information
- `GET /api/v1/reputation-book/complaints` - List platform complaints
- `POST /api/v1/reputation-book/complaints/{user}` - Create complaint for user
- `GET /api/v1/reputation-book/complaints/{complaint}` - Show platform complaint
- `PUT /api/v1/reputation-book/complaints/{complaint}` - Update platform complaint
- `PATCH /api/v1/reputation-book/complaints/{complaint}/status` - Update platform complaint status
- `DELETE /api/v1/reputation-book/complaints/{complaint}` - Delete platform complaint
- `POST /api/v1/reputation-book/complaints/{complaint}/images` - Store complaint image
- `DELETE /api/v1/reputation-book/complaints/{complaint}/images/{uniqueId}` - Delete complaint image
- `POST /api/v1/reputation-book/complaints/{complaint}/add-text` - Add text to platform complaint

### Claimed Company Endpoints

- `GET /api/v1/reputation-book/claimed-company/{company}` - Show claimed company
- `POST /api/v1/reputation-book/claimed-company` - Create claimed company
- `PUT /api/v1/reputation-book/claimed-company/{company}` - Update claimed company
- `POST /api/v1/reputation-book/claimed-company/{company}/images` - Store company image
- `GET /api/v1/reputation-book/claimed-company/ruc/{param}` - Check RUC number

### Analytics Endpoints

- `GET /api/v1/reputation-book/complaints/counter` - Get complaint counters
- `GET /api/v1/reputation-book/complaints/report/{interval}` - Get complaint report

### Collaborator Endpoints

- `GET /api/v1/reputation-book/collaborators/me` - Get collaborator profile

## Common Headers

All endpoints accept the following headers:

| Header | Required | Description |
| ------ | -------- | ----------- |
| X-PUBLIC-KEY | Yes | Platform public key |
| Accept-Language | No | IETF locale (e.g., `pt-BR`, `en`, `es`) for localized content |
| Authorization | Conditional | `Bearer {token}` for authenticated endpoints |

## Complaint Status Values

- `pending` - Complaint awaiting review
- `in_progress` - Complaint being processed
- `resolved` - Complaint has been resolved
- `rejected` - Complaint was rejected

## Data Models

### Complaint

A complaint represents a user's grievance against a company, including:

- User information
- Company details
- Complaint title and description (multilingual)
- Purchase date and invoice information
- Status and visibility settings
- Attached images and evidence
- Replies and communication history

### Claimed Company

Represents a company that has been claimed or registered in the system:

- Company identification (UUID, name, slug)
- Fiscal documents (RUC, tax ID)
- Contact information
- Addresses and coverage areas
- Images and media
- Reviews and complaints

## Filtering and Pagination

Most list endpoints support:

- **Filtering**: By status, company, user, dates, and custom criteria
- **Pagination**: Standard Laravel pagination with `per_page` parameter
- **No Pagination**: Use `no_paginate=true` with optional `limit`
- **Localization**: Filter content by language

## Error Handling

All endpoints return standard error responses:

- `400` - Bad Request
- `401` - Unauthorized
- `403` - Forbidden
- `404` - Not Found
- `422` - Unprocessable Entity (validation errors)
- `429` - Too Many Requests
- `500` - Internal Server Error

## Getting Started

1. Obtain an API token and platform public key
2. Review [Endpoints Documentation](Endpoints/README.md) for detailed endpoint information
3. Use the appropriate endpoint based on your use case (public, user, or backoffice)
4. Include required headers in all requests
5. Handle responses and errors appropriately

## Related Documentation

- [Endpoints Index](Endpoints/README.md) - Complete list of all endpoints
- [Shared Domain](../Shared/README.md) - Common utilities and user management
- [Company Domain](../Company/README.md) - Company management

## Support

For questions or issues with the ReputationBook API, contact the platform support team or refer to the technical documentation.
