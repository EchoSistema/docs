# Artificial Intelligence – Process Data Calculator

## Endpoint

```
POST /api/v1/ai/admin/data/calculator/process
```

## Authentication

Required – Bearer {token} with ability `auth:sanctum`

## Headers

| Header           | Type   | Required | Description |
| ---------------- | ------ | -------- | ----------- |
| Authorization    | string | Yes      | `Bearer {token}`. |
| X-PUBLIC-KEY     | string | Yes      | Platform public key. |
| Accept-Language  | string | No       | IETF locale (e.g., `pt-BR`, `en`, `es`). |
| Content-Type     | string | Yes      | `multipart/form-data` or `application/json`. |

## Parameters

### Body parameters

| Parameter   | Type    | Required | Description | Default/Values |
| ----------- | ------- | -------- | ----------- | -------------- |
| file        | file    | Required without json | Data file to analyze. | Supported formats: CSV, TSV, TXT, JSON, NDJSON, XML, YAML, Excel (XLS, XLSX) |
| json        | string  | Required without file | JSON string data to analyze. | Valid JSON string |
| has_header  | boolean | No       | Whether the file has a header row. | false |
| delimiter   | string  | No       | Delimiter character for CSV/TSV files. | Auto-detected |
| format      | string  | No       | File format hint. | csv, tsv, dsv, json, ndjson, xml, yaml, excel |
| currency    | string  | No       | Currency code for price calculation. | Platform default currency |

> One of `file` or `json` is required.
> Parameter names accept `snake_case`, `camelCase`, `kebab-case`, or `CapitalCase` variants.

## Examples

### Request example (curl) - File upload

```bash
curl -X POST \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: en" \
  -F "file=@/path/to/data.csv" \
  -F "has_header=true" \
  -F "currency=USD" \
  "https://sandbox.your-domain.com/api/v1/ai/admin/data/calculator/process"
```

### Request example (curl) - JSON data

```bash
curl -X POST \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: en" \
  -H "Content-Type: application/json" \
  -d '{
    "json": "[{\"id\":1,\"name\":\"John\"},{\"id\":2,\"name\":\"Jane\"}]",
    "currency": "USD"
  }' \
  "https://sandbox.your-domain.com/api/v1/ai/admin/data/calculator/process"
```

### Request example (JavaScript) - File upload

```javascript
const formData = new FormData();
formData.append('file', fileInput.files[0]);
formData.append('has_header', 'true');
formData.append('currency', 'USD');

const response = await fetch('https://sandbox.your-domain.com/api/v1/ai/admin/data/calculator/process', {
  method: 'POST',
  headers: {
    'Authorization': 'Bearer <token>',
    'X-PUBLIC-KEY': '<key>',
    'Accept-Language': 'en',
  },
  body: formData
});
```

### Response example

```json
{
  "data": {
    "records": 1000,
    "json_records": [
      {"id": 1, "name": "John", "email": "john@example.com"},
      {"id": 2, "name": "Jane", "email": "jane@example.com"}
    ],
    "size": 52480,
    "price": {
      "index": 1,
      "uuid": "00000000-0000-0000-0000-000000000001",
      "value": "0.0010",
      "raw_value": 10,
      "formatted_value": "$0.0010",
      "float_value": 0.001,
      "currency": "USD",
      "starts_at": "2025-01-01",
      "finishes_at": null,
      "is_active": true,
      "is_default": true,
      "precision": 4
    },
    "total_value": {
      "value": 52,
      "float_value": 0.5248,
      "raw_value": "0.5248",
      "formatted_value": "$0.5248",
      "currency": "USD",
      "precision": 4
    }
  }
}
```

## JSON Structure Explained

| Field                          | Type    | Description |
| ------------------------------ | ------- | ----------- |
| data                           | object  | Calculation results. |
| data.records                   | integer | Number of records processed. |
| data.json_records              | array   | Parsed data records (sample or full dataset). |
| data.size                      | integer | Data size in bytes. |
| data.price                     | object  | Price information per byte. |
| data.price.index               | integer | Price index number. |
| data.price.uuid                | string  | Price UUID. |
| data.price.value               | string  | Price per byte (4 decimal places). |
| data.price.raw_value           | integer | Raw price value. |
| data.price.formatted_value     | string  | Formatted price with currency symbol. |
| data.price.float_value         | float   | Price as float. |
| data.price.currency            | string  | Currency code. |
| data.price.starts_at           | string  | Price validity start date. |
| data.price.finishes_at         | string  | Price validity end date (nullable). |
| data.price.is_active           | boolean | Whether price is active. |
| data.price.is_default          | boolean | Whether price is default. |
| data.price.precision           | integer | Decimal precision (always 4). |
| data.total_value               | object  | Total cost calculation. |
| data.total_value.value         | integer | Total value in cents. |
| data.total_value.float_value   | float   | Total value as float. |
| data.total_value.raw_value     | string  | Raw total value string. |
| data.total_value.formatted_value | string | Formatted total with currency symbol. |
| data.total_value.currency      | string  | Currency code. |
| data.total_value.precision     | integer | Decimal precision (always 4). |

## HTTP Status

- 200: OK
- 401: Unauthorized
- 403: Forbidden
- 404: Not Found (byte pricing product not configured)
- 422: Unprocessable Entity (validation errors, invalid file format)
- 429: Too Many Requests
- 500: Internal Server Error

## Errors

### Validation Error

```json
{
  "message": "The file field is required when json is not present.",
  "errors": {
    "file": [
      "The file field is required when json is not present."
    ]
  }
}
```

### Product Not Found

```json
{
  "success": false,
  "message": "Product not found"
}
```

### Price Not Found

```json
{
  "success": false,
  "message": "Byte price not found for calculation"
}
```

### Processing Error

```json
{
  "success": false,
  "message": "An unexpected error occurred"
}
```

## Notes

- This endpoint calculates the cost of processing data based on its size in bytes.
- You must provide either a `file` or a `json` parameter.
- Supported file formats: CSV, TSV, DSV, JSON, NDJSON, XML, YAML, Excel (XLS, XLSX).
- The file must have one of these MIME types: `text/plain`, `text/csv`, `application/csv`, `application/vnd.ms-excel`, `application/vnd.openxmlformats-officedocument.spreadsheetml.sheet`, `text/tab-separated-values`, `application/x-ndjson`, `application/json`, `text/xml`, `application/xml`, `application/x-yaml`, `text/yaml`, `text/x-yaml`.
- The system uses the byte pricing configured for your platform (slug: "byte_price").
- If no currency is specified, the platform's default currency is used.
- If a specific currency is requested but not available, the default price is used.
- The price per byte is calculated as: `price_value / 10000`.
- Total cost is calculated as: `price_per_byte * size_in_bytes`.
- Errors are logged to Discord for monitoring.

## Related

- [Byte Pricing Index](./BytePricingIndex.md)
- [Byte Pricing Show](./BytePricingShow.md)
- [Byte Pricing Store](./BytePricingStore.md)
- [Byte Pricing Update](./BytePricingUpdate.md)

## Changelog

- 2025-10-23: Updated with detailed documentation and accurate request/response structure.
- 2025-10-16: Initial documentation.
