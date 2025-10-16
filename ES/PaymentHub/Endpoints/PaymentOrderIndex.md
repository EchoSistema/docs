# PaymentHub – Índice de Órdenes de Pago

## Endpoint

```
GET /api/v1/payment-hub/payment-orders
```

## Autenticación

Requerida – Bearer {token}

## Encabezados

| Encabezado       | Tipo   | Requerido | Descripción |
| ---------------- | ------ | --------- | ----------- |
| Authorization    | string | Sí        | Credencial `Bearer {token}`. |
| X-PUBLIC-KEY     | string | Sí        | Clave pública de la plataforma. |

## Parámetros

### Parámetros de consulta

| Parámetro | Tipo    | Requerido | Descripción | Predeterminado/Valores |
| --------- | ------- | --------- | ----------- | ---------------------- |
| per_page  | integer | No        | Número de resultados por página | 10 (1-100) |
| page      | integer | No        | Número de página para la paginación | 1 |

> Los nombres de parámetros aceptan variantes (`camelCase`, `kebab-case`, `snake_case`).

## Ejemplos

### Ejemplo de solicitud (curl)

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <clave>" \
  "https://sandbox.su-dominio.com/api/v1/payment-hub/payment-orders?per_page=10"
```

### Ejemplo de respuesta

```json
{
  "data": [
    {
      "uuid": "550e8400-e29b-41d4-a716-446655440000",
      "status": "completed",
      "amount": 15000,
      "currency": "EUR",
      "created_at": "2025-10-15T10:30:00Z",
      "updated_at": "2025-10-15T11:00:00Z"
    }
  ],
  "links": {
    "first": "https://api.example.com/api/v1/payment-hub/payment-orders?page=1",
    "last": "https://api.example.com/api/v1/payment-hub/payment-orders?page=5",
    "prev": null,
    "next": "https://api.example.com/api/v1/payment-hub/payment-orders?page=2"
  },
  "meta": {
    "current_page": 1,
    "from": 1,
    "last_page": 5,
    "per_page": 10,
    "to": 10,
    "total": 50
  }
}
```

## Estructura JSON Explicada

| Campo              | Tipo    | Descripción |
| ------------------ | ------- | ----------- |
| data[]             | array   | Lista de órdenes de pago |
| data[].uuid        | string  | Identificador único de la orden de pago |
| data[].status      | string  | Estado actual de la orden de pago |
| data[].amount      | integer | Monto del pago en centavos |
| data[].currency    | string  | Código de moneda ISO |
| data[].created_at  | string  | Marca de tiempo ISO 8601 de creación |
| data[].updated_at  | string  | Marca de tiempo ISO 8601 de última actualización |
| links              | object  | Enlaces de paginación |
| meta               | object  | Metadatos de paginación |

## Estados HTTP

- 200: OK
- 401: No autorizado
- 403: Prohibido
- 429: Demasiadas solicitudes
- 500: Error interno del servidor

## Errores

Respuesta de error estándar:

```json
{
  "message": "Unauthenticated."
}
```

## Notas

- Las órdenes de pago se filtran por la plataforma autenticada
- Los resultados están paginados con un valor predeterminado de 10 elementos por página
- El valor máximo de per_page es 100
- Las órdenes se devuelven en orden descendente por fecha de creación

## Relacionados

- [Mostrar Orden de Pago](./PaymentOrderShow.md)
- [Dominio PaymentHub](../README.md)

## Changelog

- 2025-10-16: Documentación inicial
