# Inteligencia Artificial – Crear Producto de Precio por Byte

## Endpoint

```
POST /api/v1/ai/admin/pricing/bytes
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

### Parámetros del cuerpo

| Parámetro | Tipo    | Requerido | Descripción | Por defecto/Valores |
| --------- | ------- | -------- | ----------- | -------------- |
| price     | entero | Sí      | Price per byte (raw value * 10000). For example, 10 represents $0.0010 | Min: 0 |
| currency  | cadena  | Sí      | Currency code (ISO 4217). | Valid currency names from CurrencyEnum |
| description | cadena | No     | Product description. | Max: 255 characters |
| language  | cadena  | No       | Language code for the title. | Platform default language |

> Parámetro names accept `snake_case`, `camelCase`, `kebab-case`, or `CapitalCase` variants.

## Ejemplos

### Ejemplo de solicitud (curl)

```bash
curl -X POST \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: en" \
  -H "Content-Type: application/json" \
  -d '{
    "price": 10,
    "currency": "USD",
    "description": "Price per byte for data processing and storage",
    "language": "en"
  }' \
  "https://sandbox.your-domain.com/api/v1/ai/admin/pricing/bytes"
```

### Ejemplo de solicitud (JavaScript)

```javascript
const response = await fetch('https://sandbox.your-domain.com/api/v1/ai/admin/pricing/bytes', {
  method: 'POST',
  headers: {
    'Authorization': 'Bearer <token>',
    'X-PUBLIC-KEY': '<key>',
    'Accept-Language': 'en',
    'Content-Type': 'application/json',
  },
  body: JSON.stringify({
    price: 10,
    currency: 'USD',
    description: 'Price per byte for data processing and storage',
    language: 'en'
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
    "description": "Price per byte for data processing and storage",
    "language": "en",
    "price": "0.0010",
    "raw_price": 10,
    "price_precision": 4,
    "prices": [],
    "currency": "USD",
    "formatted_price": "$0.0010",
    "created_at": "2025-10-23T14:30:00Z"
  }
}
```

## Estructura JSON Explicada

| Campo                          | Tipo    | Descripción |
| ------------------------------ | ------- | ----------- |
| data                           | objeto  | Created byte pricing product. |
| data.uuid                      | cadena  | UUID del producto. |
| data.measurement_type          | objeto  | Measurement type information. |
| data.measurement_type.id       | entero | Measurement type ID. |
| data.measurement_type.name     | cadena  | Measurement type name (BYTE). |
| data.measurement_type.title    | cadena  | Human-readable measurement type. |
| data.title                     | cadena  | Product title (always "Byte Price"). |
| data.slug                      | cadena  | URL-friendly identifier (always "byte_price"). |
| data.description               | cadena  | Product description. |
| data.language                  | cadena  | Content language code. |
| data.price                     | cadena  | Price per byte (formatted with 4 decimal places). |
| data.raw_price                 | entero | Raw price value (price * 10000). |
| data.price_precision           | entero | Decimal precision for price (always 4). |
| data.prices                    | matriz   | Alternative prices (empty on creation). |
| data.currency                  | cadena  | Currency code. |
| data.formatted_price           | cadena  | Formatted price with currency symbol. |
| data.created_at                | cadena  | Creation timestamp (ISO 8601). |

## Estado HTTP

- 200: OK (product already exists, returns existing product)
- 201: Created
- 401: Unauthorized
- 403: Forbidden
- 422: Unprocessable Entity (validation errors)
- 429: Too Many Requests
- 500: Internal Server Error

## Errores

### Validation Error

```json
{
  "message": "The price field is required.",
  "errors": {
    "price": [
      "The price field is required."
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

## Notas

- Este endpoint creates a byte pricing product for the authenticated user's platform.
- The title is automatically set to "Byte Price" and slug to "byte_price".
- The measurement type is automatically set to BYTE.
- If a byte pricing product already exists for the platform, the existing product is returned instead of creating a new one.
- The price parameter should be the raw value multiplied by 10000 (e.g., to set a price of $0.0010, send 10).
- The currency parameter must be a valid currency name from the CurrencyEnum (e.g., "USD", "EUR", "GBP").
- The description is optional and can be up to 255 characters.

## Relacionado

- [Byte Pricing Index](./BytePricingIndex.md)
- [Byte Pricing Show](./BytePricingShow.md)
- [Byte Pricing Update](./BytePricingUpdate.md)
- [Byte Pricing Destroy](./BytePricingDestroy.md)

## Historial de cambios

- 2025-10-23: Actualizado con documentación detallada y request/estructura de respuesta precisa.
- 2025-10-16: Documentación inicial.
