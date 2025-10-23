# Inteligencia Artificial – Listar Productos de Precios por Byte

## Endpoint

```
GET /api/v1/ai/admin/pricing/bytes
```

## Autenticación

Required – Bearer {token} with ability `auth:sanctum`

## Encabezados

| Encabezado           | Tipo   | Requerido | Descripción |
| ---------------- | ------ | -------- | ----------- |
| Authorization    | cadena | Sí      | `Bearer {token}`. |
| X-PUBLIC-KEY     | cadena | Sí      | Clave pública de la plataforma. |
| Accept-Language  | cadena | No       | Configuración regional IETF (ej., `pt-BR`, `en`, `es`). |

## Parámetros

### Parámetros de consulta

| Parámetro | Tipo    | Requerido | Descripción | Por defecto/Valores |
| --------- | ------- | -------- | ----------- | -------------- |
| page      | entero | No       | Número de página | 1 |
| per_page  | entero | No       | Resultados por página | 25 (1-100) |

> Parámetro names accept `snake_case`, `camelCase`, `kebab-case`, or `CapitalCase` variants.

## Ejemplos

### Ejemplo de solicitud (curl)

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: en" \
  "https://sandbox.your-domain.com/api/v1/ai/admin/pricing/bytes?page=1&per_page=25"
```

### Ejemplo de respuesta

```json
{
  "data": [
    {
      "uuid": "00000000-0000-0000-0000-000000000001",
      "measurement_type": {
        "id": 1,
        "name": "BYTE",
        "title": "Byte"
      },
      "title": "Byte Price",
      "slug": "byte_price",
      "description": "Price per byte for data processing",
      "language": "en",
      "price": "0.0010",
      "raw_price": 10,
      "price_precision": 4,
      "prices": [
        {
          "currency_id": 2,
          "currency": "EUR",
          "value": "0.0009",
          "raw_value": 9,
          "formatted_value": "€0.0009"
        }
      ],
      "currency": "USD",
      "formatted_price": "$0.0010",
      "created_at": "2025-10-16T10:30:00Z"
    }
  ],
  "links": {
    "first": "https://sandbox.your-domain.com/api/v1/ai/admin/pricing/bytes?page=1",
    "last": "https://sandbox.your-domain.com/api/v1/ai/admin/pricing/bytes?page=1",
    "prev": null,
    "next": null
  },
  "meta": {
    "current_page": 1,
    "from": 1,
    "last_page": 1,
    "path": "https://sandbox.your-domain.com/api/v1/ai/admin/pricing/bytes",
    "per_page": 25,
    "to": 1,
    "total": 1
  }
}
```

## Estructura JSON Explicada

| Campo                          | Tipo    | Descripción |
| ------------------------------ | ------- | ----------- |
| data                           | matriz   | List of byte pricing products. |
| data[].uuid                    | cadena  | UUID del producto. |
| data[].measurement_type        | objeto  | Measurement type information. |
| data[].measurement_type.id     | entero | Measurement type ID. |
| data[].measurement_type.name   | cadena  | Measurement type name (BYTE). |
| data[].measurement_type.title  | cadena  | Human-readable measurement type. |
| data[].title                   | cadena  | Product title. |
| data[].slug                    | cadena  | URL-friendly identifier. |
| data[].description             | cadena  | Product description. |
| data[].language                | cadena  | Content language code. |
| data[].price                   | cadena  | Price per byte (formatted with 4 decimal places). |
| data[].raw_price               | entero | Raw price value (price * 10000). |
| data[].price_precision         | entero | Decimal precision for price (always 4). |
| data[].prices                  | matriz   | Alternative prices in other currencies. |
| data[].prices[].currency_id    | entero | Currency ID. |
| data[].prices[].currency       | cadena  | Currency code (ISO 4217). |
| data[].prices[].value          | cadena  | Price value in this currency. |
| data[].prices[].raw_value      | entero | Raw price value. |
| data[].prices[].formatted_value| cadena  | Formatted price with currency symbol. |
| data[].currency                | cadena  | Default currency code. |
| data[].formatted_price         | cadena  | Formatted default price with currency symbol. |
| data[].created_at              | cadena  | Creation timestamp (ISO 8601). |
| links                          | objeto  | Pagination links. |
| meta                           | objeto  | Pagination metadata. |

## Estado HTTP

- 200: OK
- 401: Unauthorized
- 403: Forbidden
- 429: Too Many Requests
- 500: Internal Server Error

## Errores

```json
{
  "message": "Unauthenticated.",
  "errors": {}
}
```

## Notas

- Este endpoint returns all products with measurement type `BYTE`.
- The price is stored as an entero representing the value * 10000, allowing for 4 decimal places of precision.
- The `formatted_price` uses the platform's locale for currency formatting.
- Pagination is set to 25 items per page by default.
- Alternative prices show active prices in currencies different from the default currency.

## Relacionado

- [Byte Pricing Show](./BytePricingShow.md)
- [Byte Pricing Store](./BytePricingStore.md)
- [Byte Pricing Update](./BytePricingUpdate.md)
- [Byte Pricing Destroy](./BytePricingDestroy.md)

## Historial de cambios

- 2025-10-23: Actualizado con documentación detallada y estructura de respuesta precisa.
- 2025-10-16: Documentación inicial.
