# PaymentHub – Mostrar Orden de Pago

## Endpoint

```
GET /api/v1/payment-hub/payment-orders/{paymentOrder}
```

## Autenticación

Requerida – Bearer {token}

## Encabezados

| Encabezado       | Tipo   | Requerido | Descripción |
| ---------------- | ------ | --------- | ----------- |
| Authorization    | string | Sí        | Credencial `Bearer {token}`. |
| X-PUBLIC-KEY     | string | Sí        | Clave pública de la plataforma. |

## Parámetros

### Parámetros de ruta

| Parámetro    | Tipo   | Requerido | Descripción |
| ------------ | ------ | --------- | ----------- |
| paymentOrder | string | Sí        | UUID de la orden de pago |

## Ejemplos

### Ejemplo de solicitud (curl)

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <clave>" \
  "https://sandbox.su-dominio.com/api/v1/payment-hub/payment-orders/550e8400-e29b-41d4-a716-446655440000"
```

### Ejemplo de respuesta

```json
{
  "data": {
    "uuid": "550e8400-e29b-41d4-a716-446655440000",
    "status": "completed",
    "amount": 15000,
    "currency": "EUR",
    "installments": [
      {
        "number": 1,
        "amount": 5000,
        "due_date": "2025-11-01",
        "status": "paid"
      },
      {
        "number": 2,
        "amount": 5000,
        "due_date": "2025-12-01",
        "status": "pending"
      },
      {
        "number": 3,
        "amount": 5000,
        "due_date": "2026-01-01",
        "status": "pending"
      }
    ],
    "created_at": "2025-10-15T10:30:00Z",
    "updated_at": "2025-10-15T11:00:00Z"
  }
}
```

## Estructura JSON Explicada

| Campo                    | Tipo    | Descripción |
| ------------------------ | ------- | ----------- |
| data                     | object  | Detalles de la orden de pago |
| data.uuid                | string  | Identificador único de la orden de pago |
| data.status              | string  | Estado actual de la orden de pago |
| data.amount              | integer | Monto total del pago en centavos |
| data.currency            | string  | Código de moneda ISO |
| data.installments[]      | array   | Lista de cuotas de pago |
| data.installments[].number | integer | Número de cuota |
| data.installments[].amount | integer | Monto de la cuota en centavos |
| data.installments[].due_date | string | Fecha de vencimiento de la cuota |
| data.installments[].status | string | Estado de la cuota |
| data.created_at          | string  | Marca de tiempo ISO 8601 de creación |
| data.updated_at          | string  | Marca de tiempo ISO 8601 de última actualización |

## Estados HTTP

- 200: OK
- 401: No autorizado
- 403: Prohibido
- 404: No encontrado
- 429: Demasiadas solicitudes
- 500: Error interno del servidor

## Errores

Respuestas de error estándar:

```json
{
  "message": "Payment order not found."
}
```

```json
{
  "message": "Unauthenticated."
}
```

## Notas

- La orden de pago debe pertenecer a la plataforma autenticada
- Los datos de cuotas se cargan automáticamente al ver una orden de pago específica
- Si la orden de pago no existe o no pertenece a la plataforma, se devuelve un error 404

## Relacionados

- [Índice de Órdenes de Pago](./PaymentOrderIndex.md)
- [Dominio PaymentHub](../README.md)

## Changelog

- 2025-10-16: Documentación inicial
