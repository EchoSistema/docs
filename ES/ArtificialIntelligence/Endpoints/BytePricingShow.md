# Inteligencia Artificial – Mostrar Detalles del Precio por Byte

## Endpoint

```
GET /api/v1/ai/admin/pricing/bytes/details
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

No parameters required.

## Ejemplos

### Ejemplo de solicitud (curl)

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: en" \
  "https://sandbox.your-domain.com/api/v1/ai/admin/pricing/bytes/details"
```

### Ejemplo de respuesta

```json
{
  "data": {
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
      },
      {
        "currency_id": 3,
        "currency": "GBP",
        "value": "0.0008",
        "raw_value": 8,
        "formatted_value": "£0.0008"
      }
    ],
    "currency": "USD",
    "formatted_price": "$0.0010",
    "created_at": "2025-10-16T10:30:00Z"
  }
}
```

## Estructura JSON Explicada

| Campo                          | Tipo    | Descripción |
| ------------------------------ | ------- | ----------- |
| data                           | objeto  | Byte pricing product details. |
| data.uuid                      | cadena  | UUID del producto. |
| data.measurement_type          | objeto  | Measurement type information. |
| data.measurement_type.id       | entero | Measurement type ID. |
| data.measurement_type.name     | cadena  | Measurement type name (BYTE). |
| data.measurement_type.title    | cadena  | Human-readable measurement type. |
| data.title                     | cadena  | Product title. |
| data.slug                      | cadena  | URL-friendly identifier (always "byte_price"). |
| data.description               | cadena  | Product description. |
| data.language                  | cadena  | Content language code. |
| data.price                     | cadena  | Price per byte (formatted with 4 decimal places). |
| data.raw_price                 | entero | Raw price value (price * 10000). |
| data.price_precision           | entero | Decimal precision for price (always 4). |
| data.prices                    | matriz   | Alternative prices in other currencies. |
| data.prices[].currency_id      | entero | Currency ID. |
| data.prices[].currency         | cadena  | Currency code (ISO 4217). |
| data.prices[].value            | cadena  | Price value in this currency. |
| data.prices[].raw_value        | entero | Raw price value. |
| data.prices[].formatted_value  | cadena  | Formatted price with currency symbol. |
| data.currency                  | cadena  | Default currency code. |
| data.formatted_price           | cadena  | Formatted default price with currency symbol. |
| data.created_at                | cadena  | Creation timestamp (ISO 8601). |

## Estado HTTP

- 200: OK
- 401: Unauthorized
- 403: Forbidden
- 404: Not Found (when byte pricing product doesn't exist for the platform)
- 429: Too Many Requests
- 500: Internal Server Error

## Errores

### Not Found

```json
{
  "message": "The requested resource was not found.",
  "errors": {}
}
```

### Unauthenticated

```json
{
  "message": "Unauthenticated.",
  "errors": {}
}
```

## Notas

- Este endpoint returns the byte pricing product associated with the authenticated user's platform.
- The product is identified by the slug "byte_price" and measurement type BYTE.
- Returns 404 if no byte pricing product exists for the platform.
- The price represents the cost per byte of data processing.
- The `formatted_price` uses the platform's locale for currency formatting.
- Alternative prices show all active prices in currencies different from the default currency.

## Relacionado

- [Byte Pricing Index](./BytePricingIndex.md)
- [Byte Pricing Store](./BytePricingStore.md)
- [Byte Pricing Update](./BytePricingUpdate.md)
- [Byte Pricing Destroy](./BytePricingDestroy.md)
- [Data Calculator Process](./DataCalculatorProcess.md)

## Historial de cambios

- 2025-10-23: Actualizado con documentación detallada y estructura de respuesta precisa.
- 2025-10-16: Documentación inicial.
