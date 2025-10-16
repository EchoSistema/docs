# Bio – Skills Eliminar

## Endpoint

```
DELETE /api/v1/bio/skills/{uuid}
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
| uuid      | string | Yes      | Skill UUID. |

## Ejemplos

### Ejemplo de solicitud (curl)

```bash
curl -X DELETE \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: en" \
  "https://sandbox.your-domain.com/api/v1/bio/skills/550e8400-e29b-41d4-a716-446655440000"
```

### Ejemplo de respuesta

```json
{
  "success": true,
  "message": "Skill 'Python' deleted successfully."
}
```

## JSON Structure Explained

| Field   | Tipo    | Descripción |
| ------- | ------- | ----------- |
| success | boolean | Operation success status. |
| message | string  | Success message with skill name. |

## Estados HTTP

- 200: OK
- 401: No autorizado
- 403: Prohibido
- 404: No encontrado
- 429: Demasiadas solicitudes
- 500: Error interno del servidor

## Errores

Not found error example:

```json
{
  "message": "Skill not found."
}
```

Forbidden error example:

```json
{
  "message": "You are not authorized to delete this skill."
}
```

## Notas

- Only the creator of the skill can delete it.
- Deletion is permanent and cannot be undone.
- If the skill is referenced by user biographies, consider the cascade behavior.

## Relacionados

- [Skills — Índice](SkillÍndice.md)
- [Skills — Crear](SkillCrear.md)
- [Skills — Batch Eliminar](SkillBatchEliminar.md)
