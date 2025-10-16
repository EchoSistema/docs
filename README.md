# EchoSistema API Documentation

Welcome to the EchoSistema Backoffice API documentation. This comprehensive documentation covers all API endpoints across multiple domains in three languages.

## Quick Start

Each platform must include the `X-PUBLIC-KEY` header to authenticate and receive responses.
The frontend should send the current application language via the `Accept-Language` header (e.g., `en`, `pt-BR`, `es`).

## Available Languages

- [English (EN)](./EN/) - Complete documentation in English
- [Español (ES)](./ES/) - Documentación completa en español
- [Português (PT)](./PT/) - Documentação completa em português

## Documented Domains

This documentation covers **17 domains** with **251 endpoints** across **3 languages**:

### Core Domains (Largest)

1. **[Shared](./EN/Shared/README.md)** - Core shared functionality, authentication, users, chat, and platform management (68 endpoints)
2. **[Bio](./EN/Bio/README.md)** - Professional biographies, education, experience, skills, and portfolios (67 endpoints)

### Business Domains

3. **[RealEstate](./EN/RealEstate/README.md)** - Real estate properties, agents, types, and rental listings (20 endpoints)
4. **[ReputationBook](./EN/ReputationBook/README.md)** - Complaint management, company reviews, and reputation tracking (16 endpoints)
5. **[Learn](./EN/Learn/README.md)** - Educational events, courses, content, and learning materials (13 endpoints)
6. **[Company](./EN/Company/README.md)** - Company profiles, reviews, job opportunities, and coverage (10 endpoints)
7. **[Ecommerce](./EN/Ecommerce/README.md)** - Product management, inventory, and resale functionality (10 endpoints)
8. **[Rent](./EN/Rent/README.md)** - Rentable item management and rental services (8 endpoints)

### Supporting Domains

9. **[Affiliate](./EN/Affiliate/README.md)** - Affiliate relationships and platform affiliations (6 endpoints)
10. **[ArtificialIntelligence](./EN/ArtificialIntelligence/README.md)** - AI byte pricing and data calculation services (6 endpoints)
11. **[Microservices](./EN/Microservices/README.md)** - Public country and language services (6 endpoints)
12. **[Articles](./EN/Articles/README.md)** - Article and author content management (5 endpoints)
13. **[Auth](./EN/Auth/README.md)** - Authentication, login, logout, and registration (4 endpoints)
14. **[Cms](./EN/Cms/README.md)** - Content management for titles and texts (4 endpoints)
15. **[Development](./EN/Development/README.md)** - Mock endpoints for development and testing (3 endpoints)
16. **[Footprints](./EN/Footprints/README.md)** - User activity tracking and analytics (3 endpoints)
17. **[PaymentHub](./EN/PaymentHub/README.md)** - Payment order management and processing (2 endpoints)

## Standards and Templates

- Documentation style guide: [STYLEGUIDE.md](./STYLEGUIDE.md)
- Endpoint templates: [_templates/](./templates/) directory
  - `endpoint.en.md` - English template
  - `endpoint.es.md` - Spanish template
  - `endpoint.pt.md` - Portuguese template

Para novos endpoints, copie o template no idioma desejado e siga o `STYLEGUIDE.md` para manter consistência entre domínios e idiomas.

## Documentation Structure

Each domain follows this structure:
```
{EN|ES|PT}/
  {Domain}/
    README.md                    # Domain overview
    Endpoints/
      README.md                  # Endpoints index
      {EndpointName}.md          # Individual endpoint docs
```

## Common Information

### Authentication
Most endpoints require Bearer token authentication:
```
Authorization: Bearer {token}
X-PUBLIC-KEY: {platform_key}
```

### Pagination
List endpoints support pagination:
- `per_page` - Results per page (default: 10, max: 100)
- `page` - Page number (default: 1)

### Error Responses
All errors follow a consistent format with appropriate HTTP status codes (400, 401, 403, 404, 422, 429, 500).

## Statistics

- **Total Domains**: 17
- **Total Endpoints**: 251
- **Languages**: 3 (English, Spanish, Portuguese)
- **Total Documentation Files**: 753+ (251 endpoints × 3 languages)
- **Last Updated**: 2025-10-16

## Additional Resources

- [Documentation Manifest](../DOCUMENTATION_MANIFEST.md) - Detailed planning document
- [Completion Report](../DOCUMENTATION_COMPLETE.md) - Full completion summary

---

For detailed endpoint information, navigate to the appropriate language and domain directory.
