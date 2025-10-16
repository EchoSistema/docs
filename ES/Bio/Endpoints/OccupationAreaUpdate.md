# Bio – Occupation Areas Actualizar

## Endpoint

```
PUT /api/v1/bio/occupation-areas/{uuid}
```

## Autenticación

Requerida – Bearer {token} con habilidad `backoffice` y permiso `domain:bio`.

## Encabezados

| Encabezado           | Tipo   | Requerido | Descripción |
| ---------------- | ------ | -------- | ----------- |
| Authorization    | string | Yes      | `Bearer {token}`. |
| X-PUBLIC-KEY     | string | Yes      | Clave pública de la plataforma. |
| Accept-Language  | string | No       | Locale IETF (e.g., `pt-BR`, `en`, `es`). |
| Content-Type     | string | Yes      | `application/json`. |

## Parámetros

### Parámetros de ruta

| Parámetro | Tipo   | Requerido | Descripción |
| --------- | ------ | -------- | ----------- |
| uuid      | string | Yes      | Occupation area UUID. |

### Cuerpo de la solicitud

```json
{
  "titles": [
    {
      "content": "Software Engineering",
      "language": "en"
    }
  ]
}
```

| Field             | Tipo   | Requerido | Descripción |
| ----------------- | ------ | -------- | ----------- |
| titles            | array  | Yes      | Array of titles in different languages. |
| titles[].content  | string | Yes      | Title text. |
| titles[].language | string | Yes      | Language code (e.g., `en`, `pt-BR`). |

> Los parámetros aceptan camelCase, snake_case, kebab-case, or CapitalCase variants.

## Ejemplos

### Ejemplo de solicitud (curl)

```bash
curl -X PUT \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Content-Type: application/json" \
  -d '{
    "titles": [
      {"content": "Software Engineering", "language": "en"}
    ]
  }' \
  "https://sandbox.your-domain.com/api/v1/bio/occupation-areas/550e8400-e29b-41d4-a716-446655440000"
```

### Ejemplo de respuesta

```json
{
  "data": {
    "id": 1,
    "uuid": "550e8400-e29b-41d4-a716-446655440000",
    "titles": [
      {
        "id": 10,
        "content": "Software Engineering",
        "language": "en"
      }
    ],
    "created_at": "2025-01-15T10:30:00.000000Z",
    "updated_at": "2025-01-15T12:00:00.000000Z"
  }
}
```

## JSON Structure Explained

| Field                  | Tipo    | Descripción |
| ---------------------- | ------- | ----------- |
| data                   | object  | Actualizard occupation area. |
| data.id                | integer | Occupation area identifier. |
| data.uuid              | string  | Unique occupation area identifier. |
| data.titles[]          | array   | Multi-language titles. |
| data.created_at        | string  | Creation timestamp. |
| data.updated_at        | string  | Last update timestamp. |

## Estados HTTP

- 200: OK
- 401: No autorizado
- 403: Prohibido
- 404: No encontrado
- 422: Entidad no procesable
- 429: Demasiadas solicitudes
- 500: Error interno del servidor

## Errores

Forbidden error example:

```json
{
  "message": "You are not authorized to update this occupation area."
}
```

## Notas

- Only the creator can update the occupation area.
- Existing titles can be updated and new translations can be added.

## Relacionados

- [Occupation Areas — Índice](OccupationAreaÍndice.md)
- [Occupation Areas — Crear](OccupationAreaCrear.md)
- [Occupation Areas — Eliminar](OccupationAreaEliminar.md)
