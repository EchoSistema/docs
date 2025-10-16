# ReputationBook – Listar Estados de Quejas

## Endpoint

```
GET /api/v1/complaints/statuses
```

## Autenticación

Ninguna

## Encabezados

| Encabezado       | Tipo   | Requerido | Descripción |
| ---------------- | ------ | --------- | ----------- |
| X-PUBLIC-KEY     | string | Sí        | Clave pública de la plataforma. |
| Accept-Language  | string | No        | Locale IETF (ej.: `pt-BR`, `en`, `es`). |

## Parámetros

No se requieren parámetros.

## Ejemplos

### Ejemplo de solicitud (curl)

```bash
curl -X GET \
  -H "X-PUBLIC-KEY: <clave>" \
  -H "Accept-Language: es" \
  "https://sandbox.su-dominio.com/api/v1/complaints/statuses"
```

### Ejemplo de respuesta

```json
{
  "data": [
    {
      "value": "pending",
      "name": "PENDING",
      "title": "Pendiente"
    },
    {
      "value": "in_progress",
      "name": "IN_PROGRESS",
      "title": "En Progreso"
    },
    {
      "value": "resolved",
      "name": "RESOLVED",
      "title": "Resuelto"
    },
    {
      "value": "rejected",
      "name": "REJECTED",
      "title": "Rechazado"
    }
  ]
}
```

## Estructura JSON Explicada

| Campo       | Tipo    | Descripción |
| ----------- | ------- | ----------- |
| data[]      | array   | Lista de estados de quejas disponibles. |
| data[].value | string | El valor del estado usado en solicitudes API. |
| data[].name  | string | El nombre constante interno del estado. |
| data[].title | string | Título localizado para mostrar el estado. |

## Estados HTTP

- 200: OK

## Errores

No hay casos de error específicos para este endpoint.

## Notas

- Este endpoint retorna todas las opciones de estado de queja disponibles.
- El campo `title` está localizado según el encabezado `Accept-Language`.
- No se requiere autenticación para recuperar la lista de estados.

## Relacionados

- [Índice de Quejas](ComplaintIndex.md)
- [Actualizar Estado de Queja](ComplaintUpdateStatus.md)
