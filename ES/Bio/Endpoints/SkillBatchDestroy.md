# Bio – Skills Batch Eliminar

## Endpoint

```
DELETE /api/v1/bio/skills
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

### Cuerpo de la solicitud

```json
{
  "skills": [
    "550e8400-e29b-41d4-a716-446655440000",
    "660e8400-e29b-41d4-a716-446655440001"
  ]
}
```

| Field    | Tipo  | Requerido | Descripción |
| -------- | ----- | -------- | ----------- |
| skills   | array | Yes      | Array of skill UUIDs to delete. |

> Los parámetros aceptan camelCase, snake_case, kebab-case, or CapitalCase variants.

## Ejemplos

### Ejemplo de solicitud (curl)

```bash
curl -X DELETE \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: en" \
  -H "Content-Type: application/json" \
  -d '{
    "skills": [
      "550e8400-e29b-41d4-a716-446655440000",
      "660e8400-e29b-41d4-a716-446655440001"
    ]
  }' \
  "https://sandbox.your-domain.com/api/v1/bio/skills"
```

### Ejemplo de respuesta

```json
{
  "success": true,
  "message": "2 skills deleted successfully."
}
```

## JSON Structure Explained

| Field   | Tipo    | Descripción |
| ------- | ------- | ----------- |
| success | boolean | Operation success status. |
| message | string  | Success message with count of deleted skills. |

## Estados HTTP

- 200: OK
- 400: Solicitud inválida
- 401: No autorizado
- 403: Prohibido
- 422: Entidad no procesable
- 429: Demasiadas solicitudes
- 500: Error interno del servidor

## Errores

Validation error example:

```json
{
  "message": "The given data was invalid.",
  "errors": {
    "skills": ["The skills field is required."]
  }
}
```

## Notas

- Only skills created by the authenticated user can be deleted.
- Skills not found or not owned by the user are silently skipped.
- The response message includes the count of successfully deleted skills.
- Deletion is permanent and cannot be undone.

## Relacionados

- [Skills — Índice](SkillÍndice.md)
- [Skills — Eliminar](SkillEliminar.md)
