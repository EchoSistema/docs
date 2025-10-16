# Bio – Occupation Areas Eliminar

## Endpoint

```
DELETE /api/v1/bio/occupation-areas/{uuid}
```

## Autenticación

Requerida – Bearer {token} con habilidad `backoffice` y permiso `domain:bio`.

## Encabezados

| Encabezado           | Tipo   | Requerido | Descripción |
| ---------------- | ------ | -------- | ----------- |
| Authorization    | string | Yes      | `Bearer {token}`. |
| X-PUBLIC-KEY     | string | Yes      | Clave pública de la plataforma. |
| Accept-Language  | string | No       | Locale IETF (e.g., `pt-BR`, `en`, `es`). |

## Parámetros

### Parámetros de ruta

| Parámetro | Tipo   | Requerido | Descripción |
| --------- | ------ | -------- | ----------- |
| uuid      | string | Yes      | Occupation area UUID. |

## Ejemplos

### Ejemplo de solicitud (curl)

```bash
curl -X DELETE \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  "https://sandbox.your-domain.com/api/v1/bio/occupation-areas/550e8400-e29b-41d4-a716-446655440000"
```

### Ejemplo de respuesta

```json
{
  "success": true,
  "message": "Occupation area 'Software Development' deleted successfully."
}
```

## JSON Structure Explained

| Field   | Tipo    | Descripción |
| ------- | ------- | ----------- |
| success | boolean | Operation success status. |
| message | string  | Success message. |

## Estados HTTP

- 200: OK
- 401: No autorizado
- 403: Prohibido
- 404: No encontrado
- 429: Demasiadas solicitudes
- 500: Error interno del servidor

## Errores

Forbidden error example:

```json
{
  "message": "You are not authorized to delete this occupation area."
}
```

## Notas

- Only the creator can delete the occupation area.
- Deletion is permanent and cannot be undone.

## Relacionados

- [Occupation Areas — Índice](OccupationAreaÍndice.md)
- [Occupation Areas — Batch Eliminar](OccupationAreaBatchEliminar.md)
