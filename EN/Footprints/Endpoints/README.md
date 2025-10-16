# Footprints Endpoints

This directory contains detailed documentation for all Footprints domain endpoints.

## Endpoint List

| Endpoint | Method | Description |
| -------- | ------ | ----------- |
| [FootprintsIndex](./FootprintsIndex.md) | GET | List all footprints with filtering options |
| [FootprintsShow](./FootprintsShow.md) | GET | Retrieve details of a specific footprint |
| [FootprintsStore](./FootprintsStore.md) | POST | Create a new footprint record |

## Common Features

### Authentication

Most endpoints require authentication:
- `Authorization: Bearer {token}`
- `X-PUBLIC-KEY: {platform_key}`

The store endpoint can be used without authentication for tracking purposes.

### Filtering

Index endpoint supports:
- Filtering by date range, user, platform
- Pagination with configurable page size
- Hiding sensitive footprints (masters only)

## Related

- [Footprints Domain](../README.md)
