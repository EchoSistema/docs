# API Documentation Style Guide

This guide defines how to write and organize documentation in `docs/` to keep consistency across domains, languages, and endpoints.

## Goals
- Standardize structure, sections, naming, and language.
- Make navigation and maintenance easier, with per-domain/language indexes.
- Ensure examples and tables are comparable across endpoints.

## Folder Structure
```
docs/
  EN/ ES/ PT/            # Languages first
    <Domain>/            # e.g., Auth, Shared, Company, IA, Pigeons
      README.md          # Domain description in that language
      Endpoints/         # Endpoint documents
        README.md        # Domain endpoint index
        <Resource><Action>.md
  Postman/               # Postman collections per domain
  _templates/            # Ready-to-use templates for new docs
  README.md              # Overview and common headers
  STYLEGUIDE.md          # This document
```

## File Naming
- Endpoints: `<Resource><Action>.md` in PascalCase, e.g., `PlatformUserIndex.md`, `RoleIndex.md`, `FootprintsShow.md`.
- Indexes: `README.md` in domain folders and in `Endpoints/`.
- Avoid spaces and special characters in file names.

## Languages
- Keep PT, EN, and ES in separate folders (`PT/`, `EN/`, `ES/`).
- The structure (titles, sections, and order) must be identical across languages.
- Translations must preserve technical terms (e.g., headers `X-PUBLIC-KEY`, `Accept-Language`, ability `backoffice`).

## Endpoint Document Structure
Recommended section order (all sections below are H2 `##`):

1. Title (H1): `# <Domain> – <Endpoint Title>`
2. Endpoint
   - One or more method/route entries in a code block: `GET /api/v1/...`
   - If there are variants, list them all under the same block or multiple blocks.
3. Authentication
   - “Required – Bearer {token} with ability X” or “None”.
4. Headers
   - Table for `Authorization`, `X-PUBLIC-KEY`, `Accept-Language` (when applicable).
5. Parameters
   - Subdivide when needed: “Path parameters”, “Query parameters”, “Request body”.
   - Tables with columns: Parameter | Type | Required | Description | Default/Values (when useful).
   - Document the canonical name (suggestion: snake_case) and mention that the endpoint accepts variations (`camelCase`, `kebab-case`, `CapitalCase`) when applicable.
6. Examples
   - Request: optionally a `curl` snippet and/or JSON body.
   - Response: full example JSON and then “Explained JSON Structure”.
7. Explained JSON Structure
   - Table listing main fields and types; prioritize relevant fields.
8. HTTP Status
   - Expected codes (e.g., 200, 201, 400, 401, 403, 404, 422, 429, 500) with a brief description.
9. Errors
   - Domain-specific messages/codes (when they exist) and example error payloads.
10. Notes
   - Business rules, defaults, pagination/sorting specifics.
11. Related
   - Relative links to related endpoints.
12. Changelog (optional)
   - Short items with date/version: “2025-08-31: adds parameter X (non-breaking)”.

## Style Conventions
- A single H1 per file. The remaining sections are H2+.
- Code and keys/tokens always in backticks: `X-PUBLIC-KEY`, `Accept-Language`, `backoffice`.
- Keep tables concise; break into subsections for large parameter groups.
- Formatted JSON, with ordered keys and minimal yet realistic examples.
- Pagination: when applicable, document `links` and `meta` (first/last/prev/next, current_page, per_page, total…).

## Authentication and Common Headers
- `Authorization`: `Bearer {token}` when required.
- `X-PUBLIC-KEY`: platform-required.
- `Accept-Language`: IETF locale (`pt-BR`, `en`, `es`) for translatable fields.

## Links and References
- Use relative links within `docs/`.
- Avoid nonexistent paths (e.g., `docs/domain/...`). If referencing code, prefer real repository paths or explain in text.

## Templates
- Use the templates in `docs/_templates/` to start new endpoint files.
- Copy the file for the desired language and fill in the placeholders.

## Review Checklist
- [ ] File name follows `<Resource><Action>.md`.
- [ ] Sections are in the recommended order with correct H1/H2 levels.
- [ ] Parameter tables are complete and consistent.
- [ ] Request/response examples are valid.
- [ ] Relative links work.
- [ ] Languages keep the same structure.

---

Questions or suggestions? Update this guide and cite existing examples (e.g., `docs/PT/Shared/Endpoints/RoleIndex.md`).
