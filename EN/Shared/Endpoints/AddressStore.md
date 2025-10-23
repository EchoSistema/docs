# Shared – Create Address

## Endpoint

```
POST /api/v1/addresses
```

## Authentication

Required – Bearer {token} with ability `backoffice`

## Headers

| Header           | Type   | Required | Description |
| ---------------- | ------ | -------- | ----------- |
| Authorization    | string | Yes      | `Bearer {token}`. |
| X-PUBLIC-KEY     | string | Yes      | Platform public key. |
| Accept-Language  | string | No       | IETF locale (e.g., `pt-BR`, `en`, `es`). |
| Content-Type     | string | Yes      | `application/json`. |

## Parameters

### Body parameters

| Parameter    | Type   | Required | Description | Default/Values |
| ------------ | ------ | -------- | ----------- | -------------- |
| type         | string | No       | Address type. | Default: `BOTH`. Valid values: `BILLING`, `SHIPPING`, `FISCAL`, `BOTH` |
| uuid         | string | No       | Custom UUID for the address. | Auto-generated if not provided |
| city         | string/object | Yes | City name or object. | - |
| state        | string/object | No  | State name or object. | - |
| country_code | string | Yes      | Country code (ISO 3166-1 alpha-2). | Max: 2 characters |
| address_one  | string | Yes      | Primary address line. | - |
| address_two  | string | No       | Secondary address line. | - |
| address_three| string | No       | Tertiary address line. | - |
| address_four | string | No       | Quaternary address line. | - |
| zipcode      | string | No       | Postal/ZIP code. | - |

> Parameter names accept `snake_case`, `camelCase`, `kebab-case`, or `CapitalCase` variants.

## Examples

### Request example (curl)

```bash
curl -X POST \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: en" \
  -H "Content-Type: application/json" \
  -d '{
    "type": "BOTH",
    "city": "New York",
    "country_code": "US",
    "address_one": "350 Fifth Avenue",
    "address_two": "Floor 34",
    "zipcode": "10118"
  }' \
  "https://sandbox.your-domain.com/api/v1/addresses"
```

### Request example (JavaScript)

```javascript
const response = await fetch('https://sandbox.your-domain.com/api/v1/addresses', {
  method: 'POST',
  headers: {
    'Authorization': 'Bearer <token>',
    'X-PUBLIC-KEY': '<key>',
    'Accept-Language': 'en',
    'Content-Type': 'application/json',
  },
  body: JSON.stringify({
    type: 'BOTH',
    city: 'New York',
    country_code: 'US',
    address_one: '350 Fifth Avenue',
    address_two: 'Floor 34',
    zipcode: '10118'
  })
});
```

### Response example

```json
{
  "data": {
    "uuid": "00000000-0000-0000-0000-000000000001",
    "main": true,
    "type": "BOTH",
    "zipcode": "10118",
    "one": "350 Fifth Avenue",
    "two": "Floor 34",
    "three": null,
    "four": null,
    "city": {
      "uuid": "00000000-0000-0000-0000-000000000002",
      "name": "New York",
      "abbreviation": "NY"
    },
    "state": {
      "uuid": "00000000-0000-0000-0000-000000000003",
      "name": "New York",
      "abbreviation": "NY",
      "region": "Northeast"
    },
    "country": {
      "uuid": "00000000-0000-0000-0000-000000000004",
      "name": "United States",
      "official_name": "United States of America",
      "code": "US"
    },
    "formatted": "350 Fifth Avenue, Floor 34, New York, NY 10118, United States"
  }
}
```

## JSON Structure Explained

| Field                   | Type    | Description |
| ----------------------- | ------- | ----------- |
| data                    | object  | Created address. |
| data.uuid               | string  | Address UUID. |
| data.main               | boolean | Whether this is the main address (true when type is `BOTH`). |
| data.is_billing         | boolean | Whether this is a billing address (present when type is not `BOTH`). |
| data.is_shipping        | boolean | Whether this is a shipping address (present when type is not `BOTH`). |
| data.is_fiscal          | boolean | Whether this is a fiscal address (present when type is not `BOTH`). |
| data.type               | string  | Address type. |
| data.zipcode            | string  | Postal/ZIP code. |
| data.one                | string  | Primary address line. |
| data.two                | string  | Secondary address line. |
| data.three              | string  | Tertiary address line. |
| data.four               | string  | Quaternary address line. |
| data.city               | object  | City information. |
| data.city.uuid          | string  | City UUID. |
| data.city.name          | string  | City name. |
| data.city.abbreviation  | string  | City abbreviation. |
| data.state              | object  | State/Province information. |
| data.state.uuid         | string  | State UUID. |
| data.state.name         | string  | State name. |
| data.state.abbreviation | string  | State abbreviation. |
| data.state.region       | string  | State region. |
| data.country            | object  | Country information. |
| data.country.uuid       | string  | Country UUID. |
| data.country.name       | string  | Country name. |
| data.country.official_name | string | Country official name. |
| data.country.code       | string  | Country code (ISO 3166-1 alpha-2). |
| data.formatted          | string  | Formatted address string. |

## HTTP Status

- 200: OK
- 201: Created
- 401: Unauthorized
- 403: Forbidden
- 422: Unprocessable Entity (validation errors)
- 429: Too Many Requests
- 500: Internal Server Error

## Errors

### Validation Error

```json
{
  "message": "The city field is required.",
  "errors": {
    "city": [
      "The city field is required."
    ]
  }
}
```

### Unauthenticated

```json
{
  "message": "Unauthenticated.",
  "errors": {}
}
```

### Forbidden

```json
{
  "message": "This action is unauthorized.",
  "errors": {}
}
```

## Notes

- This endpoint creates a new address and requires the `backoffice` ability.
- The `type` field defaults to `BOTH` if not provided.
- The city resolution is handled through a pipeline that automatically resolves city, state, and country information.
- If `uuid` is not provided, it will be auto-generated.
- The `country_code` must be a valid ISO 3166-1 alpha-2 code (e.g., "US", "BR", "ES").

## Related

- [Address Update](./AddressUpdate.md)

## Changelog

- 2025-10-23: Initial documentation.
