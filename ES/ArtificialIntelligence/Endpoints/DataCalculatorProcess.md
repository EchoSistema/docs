# Inteligencia Artificial – Procesar Calculadora de Datos

## Endpoint

```
POST /api/v1/ai/admin/data/calculator/process
```

## Autenticación

Required – Bearer {token} with ability `auth:sanctum`

## Encabezados

| Encabezado           | Tipo   | Requerido | Descripción |
| ---------------- | ------ | -------- | ----------- |
| Authorization    | cadena | Sí      | `Bearer {token}`. |
| X-PUBLIC-KEY     | cadena | Sí      | Clave pública de la plataforma. |
| Accept-Language  | cadena | No       | Configuración regional IETF (ej., `pt-BR`, `en`, `es`). |
| Content-Type     | cadena | Sí      | `multipart/form-data` o `application/json`. |

## Parámetros

### Parámetros del cuerpo

| Parámetro   | Tipo    | Requerido | Descripción | Por defecto/Valores |
| ----------- | ------- | -------- | ----------- | -------------- |
| archivo        | archivo    | Requerido without json | Data archivo to analyze. | Supported formats: CSV, TSV, TXT, JSON, NDJSON, XML, YAML, Excel (XLS, XLSX) |
| json        | cadena  | Requerido without archivo | JSON cadena data to analyze. | Valid JSON cadena |
| has_header  | booleano | No       | Whether the archivo has a header row. | false |
| delimiter   | cadena  | No       | Delimiter character for CSV/TSV files. | Auto-detected |
| format      | cadena  | No       | File format hint. | csv, tsv, dsv, json, ndjson, xml, yaml, excel |
| currency    | cadena  | No       | Currency code for price calculation. | Platform default currency |

> One of `archivo` or `json` is required.
> Parámetro names accept `snake_case`, `camelCase`, `kebab-case`, or `CapitalCase` variants.

## Ejemplos

### Ejemplo de solicitud (curl) - File upload

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

### Ejemplo de solicitud (curl) - JSON data

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

### Ejemplo de solicitud (JavaScript) - File upload

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

### Ejemplo de respuesta

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

## Estructura JSON Explicada

| Campo                          | Tipo    | Descripción |
| ------------------------------ | ------- | ----------- |
| data                           | objeto  | Calculation results. |
| data.records                   | entero | Number of records processed. |
| data.json_records              | matriz   | Parsed data records (sample or full dataset). |
| data.size                      | entero | Data size in bytes. |
| data.price                     | objeto  | Price information per byte. |
| data.price.index               | entero | Price index number. |
| data.price.uuid                | cadena  | Price UUID. |
| data.price.value               | cadena  | Price per byte (4 decimal places). |
| data.price.raw_value           | entero | Raw price value. |
| data.price.formatted_value     | cadena  | Formatted price with currency symbol. |
| data.price.float_value         | flotante   | Price as float. |
| data.price.currency            | cadena  | Currency code. |
| data.price.starts_at           | cadena  | Price validity start date. |
| data.price.finishes_at         | cadena  | Price validity end date (nullable). |
| data.price.is_active           | booleano | Whether price is active. |
| data.price.is_default          | booleano | Whether price is default. |
| data.price.precision           | entero | Decimal precision (always 4). |
| data.total_value               | objeto  | Total cost calculation. |
| data.total_value.value         | entero | Total value in cents. |
| data.total_value.float_value   | flotante   | Total value as float. |
| data.total_value.raw_value     | cadena  | Raw total value string. |
| data.total_value.formatted_value | cadena | Formatted total with currency symbol. |
| data.total_value.currency      | cadena  | Currency code. |
| data.total_value.precision     | entero | Decimal precision (always 4). |

## Estado HTTP

- 200: OK
- 401: Unauthorized
- 403: Forbidden
- 404: Not Found (byte pricing product not configured)
- 422: Unprocessable Entity (validation errors, invalid archivo format)
- 429: Too Many Requests
- 500: Internal Server Error

## Errores

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

## Notas

- Este endpoint calculates the cost of processing data based on its size in bytes.
- You must provide either a `archivo` or a `json` parameter.
- Supported archivo formats: CSV, TSV, DSV, JSON, NDJSON, XML, YAML, Excel (XLS, XLSX).
- The archivo must have one of these MIME types: `text/plain`, `text/csv`, `application/csv`, `application/vnd.ms-excel`, `application/vnd.openxmlformats-officedocument.spreadsheetml.sheet`, `text/tab-separated-values`, `application/x-ndjson`, `application/json`, `text/xml`, `application/xml`, `application/x-yaml`, `text/yaml`, `text/x-yaml`.
- The system uses the byte pricing configured for your platform (slug: "byte_price").
- If no currency is specified, the platform's default currency is used.
- If a specific currency is requested but not available, the default price is used.
- The price per byte is calculated as: `price_value / 10000`.
- Total cost is calculated as: `price_per_byte * size_in_bytes`.
- Errores are logged to Discord for monitoring.

## Relacionado

- [Byte Pricing Index](./BytePricingIndex.md)
- [Byte Pricing Show](./BytePricingShow.md)
- [Byte Pricing Store](./BytePricingStore.md)
- [Byte Pricing Update](./BytePricingUpdate.md)

## Historial de cambios

- 2025-10-23: Actualizado con documentación detallada y request/estructura de respuesta precisa.
- 2025-10-16: Documentación inicial.
