# Company

Services for accessing company data within the EchoSistema platform.

## Overview

The Company domain provides comprehensive API endpoints for managing company information, reviews, job opportunities, and geographic coverage. This domain enables platforms to display, filter, and manage company-related data while maintaining multi-language support and platform-specific configurations.

## Key Features

- **Company Management**: Retrieve and manage company profiles with complete information
- **Address Management**: Create and manage company addresses with geolocation support
- **Category Groups**: Organize companies by business categories and domain areas
- **Company Reviews**: Enable users to rate and review companies
- **Coverage Management**: Filter and display companies based on geographic coverage
- **Job Opportunities**: Manage and display job postings with detailed requirements

## Authentication

Each platform must include the `X-PUBLIC-KEY` header to authenticate and receive responses. The frontend should send the current application language via the `Accept-Language` header (e.g., `pt-BR`, `en`, `es`).

Protected endpoints require a Bearer token with appropriate abilities (e.g., `backoffice`, `store.company.address`).

## Common Headers

- `X-PUBLIC-KEY`: Platform public key (required for all endpoints)
- `Accept-Language`: IETF locale for translatable content (optional, e.g., `pt-BR`, `en`, `es`)
- `Authorization`: Bearer token for authenticated endpoints (required for protected routes)

## Endpoints

For detailed endpoint documentation, see:

- [Endpoints Directory](./Endpoints/README.md)

## Resources

The Company domain includes the following main resources:

- **Company**: Company profile with platform integration
- **CompanyAddress**: Physical and billing addresses
- **CompanyReview**: User reviews and ratings
- **CategoryGroup**: Business category classifications
- **JobOpportunity**: Job postings and openings

## Related Domains

- **Shared**: Provides common resources like addresses, categories, and platforms
- **Bio**: Occupation areas and professional information

## Notes

- All translatable fields support multiple languages with fallback to platform default
- Geographic data includes country, state, and city with geolocation coordinates
- Company ratings are calculated automatically from user reviews
- Job opportunities support remote, hybrid, and on-site work modes
