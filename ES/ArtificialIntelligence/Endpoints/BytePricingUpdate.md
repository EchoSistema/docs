# Inteligencia Artificial – Actualizar Producto de Precio por Byte

## Endpoint

```
PUT /api/v1/ai/admin/pricing/bytes/{product:uuid}
```

## Autenticación

Required – Bearer {token} with ability `auth:sanctum`

## Encabezados

| Encabezado           | Tipo   | Requerido | Descripción |
| ---------------- | ------ | -------- | ----------- |
| Authorization    | cadena | Sí      | `Bearer {token}`. |
| X-PUBLIC-KEY     | cadena | Sí      | Clave pública de la plataforma. |
| Accept-Language  | cadena | No       | Configuración regional IETF (ej., `pt-BR`, `en`, `es`). |
| Content-Type     | cadena | Sí      | `application/json`. |

## Parámetros

### Parámetros de ruta

| Parámetro | Tipo   | Requerido | Descripción |
| --------- | ------ | -------- | ----------- |
| product   | cadena | Sí      | UUID del producto. |

### Parámetros del cuerpo

| Parámetro   | Tipo    | Requerido | Descripción | Por defecto/Valores |
| ----------- | ------- | -------- | ----------- | -------------- |
| price       | entero | No       | Price per byte (raw value * 10000). | Min: 0 |
| currency    | cadena  | Requerido with price | Currency code (ISO 4217). | Valid currency names from CurrencyEnum |
| description | cadena  | No       | Product description. | Max: 5000 characters |

> Parámetro names accept `snake_case`, `camelCase`, `kebab-case`, or `CapitalCase` variants.

## Ejemplos

### Ejemplo de solicitud (curl)

```bash
curl -X PUT \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: en" \
  -H "Content-Type: application/json" \
  -d '{
    "price": 15,
    "currency": "USD",
    "description": "Updated price per byte for enhanced data processing"
  }' \
  "https://sandbox.your-domain.com/api/v1/ai/admin/pricing/bytes/00000000-0000-0000-0000-000000000001"
```

### Ejemplo de solicitud (JavaScript)

```javascript
const response = await fetch('https://sandbox.your-domain.com/api/v1/ai/admin/pricing/bytes/00000000-0000-0000-0000-000000000001', {
  method: 'PUT',
  headers: {
    'Authorization': 'Bearer <token>',
    'X-PUBLIC-KEY': '<key>',
    'Accept-Language': 'en',
    'Content-Type': 'application/json',
  },
  body: JSON.stringify({
    price: 15,
    currency: 'USD',
    description: 'Updated price per byte for enhanced data processing'
  })
});
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
    "description": "Updated price per byte for enhanced data processing",
    "language": "en",
    "price": "0.0015",
    "raw_price": 15,
    "price_precision": 4,
    "prices": [
      {
        "currency_id": 2,
        "currency": "EUR",
        "value": "0.0013",
        "raw_value": 13,
        "formatted_value": "€0.0013"
      }
    ],
    "currency": "USD",
    "formatted_price": "$0.0015",
    "created_at": "2025-10-16T10:30:00Z"
  }
}
```

## Estructura JSON Explicada

| Campo                          | Tipo    | Descripción |
| ------------------------------ | ------- | ----------- |
| data                           | objeto  | Updated byte pricing product. |
| data.uuid                      | cadena  | UUID del producto. |
| data.measurement_type          | objeto  | Measurement type information. |
| data.measurement_type.id       | entero | Measurement type ID. |
| data.measurement_type.name     | cadena  | Measurement type name (BYTE). |
| data.measurement_type.title    | cadena  | Human-readable measurement type. |
| data.title                     | cadena  | Product title. |
| data.slug                      | cadena  | URL-friendly identifier. |
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
- 404: Not Found (product not found or wrong measurement type)
- 422: Unprocessable Entity (validation errors)
- 429: Too Many Requests
- 500: Internal Server Error

## Errores

### Validation Error

```json
{
  "message": "The currency field is required when price is present.",
  "errors": {
    "currency": [
      "The currency field is required when price is present."
    ]
  }
}
```

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

- Este endpoint updates a byte pricing product.
- The product must have measurement type BYTE, otherwise 404 is returned.
- All parameters are optional, allowing partial updates.
- If you provide a price, you must also provide the currency.
- The price parameter should be the raw value multiplied by 10000 (e.g., to set a price of $0.0015, send 15).
- The description can be up to 5000 characters (increased from 255 in store).
- The currency parameter must be a valid currency name from the CurrencyEnum.

## Relacionado

- [Byte Pricing Index](./BytePricingIndex.md)
- [Byte Pricing Show](./BytePricingShow.md)
- [Byte Pricing Store](./BytePricingStore.md)
- [Byte Pricing Destroy](./BytePricingDestroy.md)

## Historial de cambios

- 2025-10-23: Actualizado con documentación detallada y request/estructura de respuesta precisa.
- 2025-10-16: Documentación inicial.
