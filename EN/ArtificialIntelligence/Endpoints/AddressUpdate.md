# Artificial Intelligence – Update Address

## Endpoint

```
PUT /api/v1/ai/admin/addresses/{address}/type/{type}
```

## Authentication

Required – Bearer {token} with ability `auth:sanctum`

## Headers

| Header           | Type   | Required | Description |
| ---------------- | ------ | -------- | ----------- |
| Authorization    | string | Yes      | `Bearer {token}`. |
| X-PUBLIC-KEY     | string | Yes      | Platform public key. |
| Accept-Language  | string | No       | IETF locale (e.g., `pt-BR`, `en`, `es`). |
| Content-Type     | string | Yes      | `application/json`. |

## URL Parameters

| Parameter | Type   | Required | Description |
| --------- | ------ | -------- | ----------- |
| address   | string | Yes      | Address UUID. |
| type      | string | Yes      | Address type to filter. Valid values: `BILLING`, `SHIPPING`, `FISCAL`, `BOTH` |

## Parameters

### Body parameters

| Parameter    | Type   | Required | Description | Default/Values |
| ------------ | ------ | -------- | ----------- | -------------- |
| type         | string | No       | Address type. | Valid values: `BILLING`, `SHIPPING`, `FISCAL`, `BOTH` |
| city         | string/object | No | City name or object. | - |
| city_id      | integer | No      | City ID. | - |
| state_id     | integer | No      | State ID. | - |
| country_id   | integer | No      | Country ID. | - |
| zipcode      | string | No       | Postal/ZIP code. | Max: 255 characters |
| address_one  | string | No       | Primary address line. | Max: 255 characters |
| address_two  | string | No       | Secondary address line. | Max: 255 characters |
| address_three| string | No       | Tertiary address line. | Max: 255 characters |
| address_four | string | No       | Quaternary address line. | Max: 255 characters |
| uuid         | -      | -        | Prohibited field. Cannot be updated. | - |

> Parameter names accept `snake_case`, `camelCase`, `kebab-case`, or `CapitalCase` variants.

## Examples

### Request example (curl)

```bash
curl -X PUT \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: en" \
  -H "Content-Type: application/json" \
  -d '{
    "address_one": "350 Fifth Avenue",
    "address_two": "Floor 35",
    "zipcode": "10118"
  }' \
  "https://sandbox.your-domain.com/api/v1/ai/admin/addresses/00000000-0000-0000-0000-000000000001/type/BOTH"
```

### Request example (JavaScript)

```javascript
const response = await fetch('https://sandbox.your-domain.com/api/v1/ai/admin/addresses/00000000-0000-0000-0000-000000000001/type/BOTH', {
  method: 'PUT',
  headers: {
    'Authorization': 'Bearer <token>',
    'X-PUBLIC-KEY': '<key>',
    'Accept-Language': 'en',
    'Content-Type': 'application/json',
  },
  body: JSON.stringify({
    address_one: '350 Fifth Avenue',
    address_two: 'Floor 35',
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
    "two": "Floor 35",
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
    "formatted": "350 Fifth Avenue, Floor 35, New York, NY 10118, United States"
  }
}
```

## JSON Structure Explained

| Field                   | Type    | Description |
| ----------------------- | ------- | ----------- |
| data                    | object  | Updated address. |
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
- 401: Unauthorized
- 403: Forbidden
- 404: Not Found
- 422: Unprocessable Entity (validation errors)
- 429: Too Many Requests
- 500: Internal Server Error

## Errors

### Validation Error

```json
{
  "message": "The uuid field is prohibited.",
  "errors": {
    "uuid": [
      "The uuid field is prohibited."
    ]
  }
}
```

### Not Found

```json
{
  "message": "Not Found"
}
```

### Unauthenticated

```json
{
  "message": "Unauthenticated.",
  "errors": {}
}
```

## Notes

- This endpoint updates an existing address for the authenticated user's platform.
- Only the provided fields will be updated; all other fields remain unchanged.
- The `uuid` field is prohibited and cannot be updated.
- The address is queried by both the UUID and the type parameter in the URL.
- The city resolution is handled through a pipeline that automatically resolves city, state, and country information when city-related fields are updated.
- You can update the city using either a string/object in the `city` field or by providing `city_id`, `state_id`, and `country_id`.

## Related

- [Address Store](./AddressStore.md)

## Changelog

- 2025-10-23: Initial documentation.
