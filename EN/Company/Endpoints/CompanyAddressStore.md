# Company – Store Company Address

## Endpoint

```
POST /api/v1/company/{addressable}/addresses
```

## Authentication

Required – Bearer {token} with ability `store.company.address`

## Headers

| Header           | Type   | Required | Description |
| ---------------- | ------ | -------- | ----------- |
| Authorization    | string | Yes      | `Bearer {token}`. |
| X-PUBLIC-KEY     | string | Yes      | Platform public key. |
| Accept-Language  | string | No       | IETF locale (e.g., `pt-BR`, `en`, `es`). |

## Parameters

### Path parameters

| Parameter    | Type   | Required | Description |
| ------------ | ------ | -------- | ----------- |
| addressable  | string | Yes      | Company UUID. |

### Request body

| Parameter       | Type   | Required | Description | Default/Values |
| --------------- | ------ | -------- | ----------- | -------------- |
| type            | string | No       | Address type: `billing`, `shipping`, or `both`. | `both` |
| city            | string | Yes      | City name or identifier. | |
| state           | string | No       | State name or identifier. | |
| country_code    | string | Yes      | ISO 3166-1 alpha-2 country code (2 characters). | |
| address_one     | string | Yes      | Primary address line. | |
| address_two     | string | No       | Secondary address line. | |
| address_three   | string | No       | Tertiary address line. | |
| address_four    | string | No       | Quaternary address line. | |
| zipcode         | string | No       | Postal code. | |

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
    "type": "both",
    "country_code": "PY",
    "state": "Central",
    "city": "Asunción",
    "address_one": "Av. España 123",
    "address_two": "c/ Boquerón",
    "zipcode": "1234"
  }' \
  "https://sandbox.your-domain.com/api/v1/company/{company-uuid}/addresses"
```

### Response example

```json
{
  "data": {
    "id": 1,
    "type": "both",
    "country": {
      "id": 1,
      "name": "Paraguay",
      "code": "PY"
    },
    "state": {
      "id": 15,
      "name": "Central"
    },
    "city": {
      "id": 234,
      "name": "Asunción"
    },
    "address_one": "Av. España 123",
    "address_two": "c/ Boquerón",
    "address_three": null,
    "address_four": null,
    "zipcode": "1234",
    "created_at": "2025-10-16T12:00:00-03:00"
  }
}
```

## JSON Structure Explained

| Field              | Type     | Description |
| ------------------ | -------- | ----------- |
| data               | object   | Address resource. |
| data.id            | integer  | Address identifier. |
| data.type          | string   | Address type: `billing`, `shipping`, or `both`. |
| data.country       | object   | Country information. |
| data.country.id    | integer  | Country identifier. |
| data.country.name  | string   | Country name. |
| data.country.code  | string   | ISO 3166-1 alpha-2 country code. |
| data.state         | object   | State/province information (when available). |
| data.state.id      | integer  | State identifier. |
| data.state.name    | string   | State name. |
| data.city          | object   | City information. |
| data.city.id       | integer  | City identifier. |
| data.city.name     | string   | City name. |
| data.address_one   | string   | Primary address line. |
| data.address_two   | string   | Secondary address line. |
| data.address_three | string   | Tertiary address line. |
| data.address_four  | string   | Quaternary address line. |
| data.zipcode       | string   | Postal code. |
| data.created_at    | datetime | Creation timestamp. |

## HTTP Status

- 201: Created
- 400: Bad Request
- 401: Unauthorized
- 403: Forbidden
- 404: Not Found
- 422: Unprocessable Entity
- 429: Too Many Requests
- 500: Internal Server Error

## Errors

```json
{
  "message": "The given data was invalid.",
  "errors": {
    "city": ["The city field is required."],
    "country_code": ["The country code field is required."],
    "address_one": ["The address one field is required."]
  }
}
```

## Notes

- The `type` parameter defaults to `both` if not provided.
- The system automatically resolves city information from the provided data.
- The authenticated user must have the `store.company.address` ability.
- The `addressable` parameter in the path must be a valid company UUID.

## Related

- [Company Show](./CompanyShow.md)
- [Company Through Coverage Index](./CompanyThroughCoverageIndex.md)

## Changelog

- 2025-10-16: Initial documentation.
