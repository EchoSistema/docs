# Inteligência Artificial – Processar Calculadora de Dados

## Endpoint

```
POST /api/v1/ai/admin/data/calculator/process
```

## Autenticação

Required – Bearer {token} with ability `auth:sanctum`

## Cabeçalhos

| Cabeçalho           | Tipo   | Obrigatório | Descrição |
| ---------------- | ------ | -------- | ----------- |
| Authorization    | texto | Sim      | `Bearer {token}`. |
| X-PUBLIC-KEY     | texto | Sim      | Chave pública da plataforma. |
| Accept-Language  | texto | Não       | Localidade IETF (ex., `pt-BR`, `en`, `es`). |
| Content-Type     | texto | Sim      | `multipart/form-data` ou `application/json`. |

## Parâmetros

### Parâmetros do corpo

| Parâmetro   | Tipo    | Obrigatório | Descrição | Padrão/Valores |
| ----------- | ------- | -------- | ----------- | -------------- |
| arquivo        | arquivo    | Obrigatório without json | Data arquivo to analyze. | Supported formats: CSV, TSV, TXT, JSON, NDJSON, XML, YAML, Excel (XLS, XLSX) |
| json        | texto  | Obrigatório without arquivo | JSON texto data to analyze. | Valid JSON texto |
| has_header  | booleano | Não       | Whether the arquivo has a header row. | false |
| delimiter   | texto  | Não       | Delimiter character for CSV/TSV files. | Auto-detected |
| format      | texto  | Não       | File format hint. | csv, tsv, dsv, json, ndjson, xml, yaml, excel |
| currency    | texto  | Não       | Currency code for price calculation. | Platform default currency |

> One of `arquivo` or `json` is required.
> Parâmetro names accept `snake_case`, `camelCase`, `kebab-case`, or `CapitalCase` variants.

## Exemplos

### Exemplo de solicitação (curl) - File upload

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

### Exemplo de solicitação (curl) - JSON data

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

### Exemplo de solicitação (JavaScript) - File upload

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

### Exemplo de resposta

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

## Estrutura JSON Explicada

| Campo                          | Tipo    | Descrição |
| ------------------------------ | ------- | ----------- |
| data                           | objeto  | Calculation results. |
| data.records                   | inteiro | Number of records processed. |
| data.json_records              | matriz   | Parsed data records (sample or full dataset). |
| data.size                      | inteiro | Data size in bytes. |
| data.price                     | objeto  | Price information per byte. |
| data.price.index               | inteiro | Price index number. |
| data.price.uuid                | texto  | Price UUID. |
| data.price.value               | texto  | Price per byte (4 decimal places). |
| data.price.raw_value           | inteiro | Raw price value. |
| data.price.formatted_value     | texto  | Formatted price with currency symbol. |
| data.price.float_value         | flutuante   | Price as float. |
| data.price.currency            | texto  | Currency code. |
| data.price.starts_at           | texto  | Price validity start date. |
| data.price.finishes_at         | texto  | Price validity end date (nullable). |
| data.price.is_active           | booleano | Whether price is active. |
| data.price.is_default          | booleano | Whether price is default. |
| data.price.precision           | inteiro | Decimal precision (always 4). |
| data.total_value               | objeto  | Total cost calculation. |
| data.total_value.value         | inteiro | Total value in cents. |
| data.total_value.float_value   | flutuante   | Total value as float. |
| data.total_value.raw_value     | texto  | Raw total value string. |
| data.total_value.formatted_value | texto | Formatted total with currency symbol. |
| data.total_value.currency      | texto  | Currency code. |
| data.total_value.precision     | inteiro | Decimal precision (always 4). |

## Status HTTP

- 200: OK
- 401: Unauthorized
- 403: Forbidden
- 404: Nãot Found (byte pricing product not configured)
- 422: Unprocessable Entity (validation errors, invalid arquivo format)
- 429: Too Many Requests
- 500: Internal Server Error

## Erros

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

### Product Nãot Found

```json
{
  "success": false,
  "message": "Product not found"
}
```

### Price Nãot Found

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

## Nãotas

- Este endpoint calculates the cost of processing data based on its size in bytes.
- You must provide either a `arquivo` or a `json` parameter.
- Supported arquivo formats: CSV, TSV, DSV, JSON, NDJSON, XML, YAML, Excel (XLS, XLSX).
- The arquivo must have one of these MIME types: `text/plain`, `text/csv`, `application/csv`, `application/vnd.ms-excel`, `application/vnd.openxmlformats-officedocument.spreadsheetml.sheet`, `text/tab-separated-values`, `application/x-ndjson`, `application/json`, `text/xml`, `application/xml`, `application/x-yaml`, `text/yaml`, `text/x-yaml`.
- The system uses the byte pricing configured for your platform (slug: "byte_price").
- If no currency is specified, the platform's default currency is used.
- If a specific currency is requested but not available, the default price is used.
- The price per byte is calculated as: `price_value / 10000`.
- Total cost is calculated as: `price_per_byte * size_in_bytes`.
- Erros are logged to Discord for monitoring.

## Relacionado

- [Byte Pricing Index](./BytePricingIndex.md)
- [Byte Pricing Show](./BytePricingShow.md)
- [Byte Pricing Store](./BytePricingStore.md)
- [Byte Pricing Update](./BytePricingUpdate.md)

## Histórico de mudanças

- 2025-10-23: Atualizado com documentação detalhada e request/estrutura de resposta precisa.
- 2025-10-16: Documentação inicial.
