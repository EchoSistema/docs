# Company Endpoints

This directory contains documentation for all Company domain API endpoints.

## Available Endpoints

### Company Management

- [**Show Company**](./CompanyShow.md) - `GET /api/v1/company`
  - Retrieve details of the authenticated company

### Company Addresses

- [**Store Company Address**](./CompanyAddressStore.md) - `POST /api/v1/company/{addressable}/addresses`
  - Create a new address for a company

### Category Groups

- [**List Category Groups**](./CompanyCategoryGroupIndex.md) - `GET /api/v1/company/category-groups`
  - List all category groups available for the platform

### Company Reviews

- [**List Company Reviews**](./CompanyReviewIndex.md) - `GET /api/v1/company/reviews`
  - List all company reviews with filtering options

- [**Create Company Review**](./CompanyReviewStore.md) - `POST /api/v1/company/reviews`
  - Create a new review for a company

### Company Coverage

- [**Index Companies Through Coverage**](./CompanyThroughCoverageIndex.md) - `GET /api/v1/company/through-coverage`
  - List companies available through platform coverage

- [**Companies Geolocations**](./CompanyThroughCoverageGeolocations.md) - `GET /api/v1/company/through-coverage/geolocations`
  - Get geolocation data for companies

- [**Companies Counter**](./CompanyThroughCoverageCounter.md) - `GET /api/v1/company/through-coverage/counter`
  - Get count of companies through coverage

### Job Opportunities

- [**List Job Opportunities**](./MeJobOpportunityIndex.md) - `GET /api/v1/company/opportunities`
  - List all job opportunities for the company

- [**Show Job Opportunity**](./MeJobOpportunityShow.md) - `GET /api/v1/company/opportunities/{jobOpportunity}`
  - Get detailed information about a specific job opportunity

## Authentication

Most endpoints require authentication via Bearer token. Public endpoints only require the `X-PUBLIC-KEY` header.

## Common Headers

All endpoints accept these common headers:

- `X-PUBLIC-KEY` - Platform public key (required for all endpoints)
- `Accept-Language` - Language preference for translatable content (optional)
- `Authorization` - Bearer token for authenticated endpoints (required for protected endpoints)

## Response Format

All endpoints return JSON responses following Laravel Resource conventions with a `data` wrapper.

Paginated endpoints include `links` and `meta` objects for navigation and pagination information.

## Related Documentation

- [Company Domain Overview](../README.md)
- [API Style Guide](../../../STYLEGUIDE.md)
