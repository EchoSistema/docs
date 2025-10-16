# Bio – Skills Crear

## Endpoint

```
POST /api/v1/bio/skills
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
  "content": "Python",
  "language": "en",
  "is_default": true
}
```

| Field      | Tipo    | Requerido | Descripción |
| ---------- | ------- | -------- | ----------- |
| content    | string  | Yes      | Skill name/title. |
| language   | string  | No       | Language code (e.g., `en`, `pt-BR`). Defaults to platform language. |
| is_default | boolean | No       | Whether this is a default skill. Defaults to false. |

> Los parámetros aceptan camelCase, snake_case, kebab-case, or CapitalCase variants.

## Ejemplos

### Ejemplo de solicitud (curl)

```bash
curl -X POST \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: en" \
  -H "Content-Type: application/json" \
  -d '{
    "content": "Python",
    "language": "en",
    "is_default": true
  }' \
  "https://sandbox.your-domain.com/api/v1/bio/skills"
```

### Ejemplo de respuesta

```json
{
  "data": {
    "id": 123,
    "uuid": "550e8400-e29b-41d4-a716-446655440000",
    "content": "Python",
    "language": "en",
    "usage": "skill",
    "is_default": true,
    "created_at": "2025-01-15T10:30:00.000000Z",
    "updated_at": "2025-01-15T10:30:00.000000Z"
  }
}
```

## JSON Structure Explained

| Field          | Tipo    | Descripción |
| -------------- | ------- | ----------- |
| data           | object  | Created skill object. |
| data.id        | integer | Skill identifier. |
| data.uuid      | string  | Unique skill identifier. |
| data.content   | string  | Skill name/title. |
| data.language  | string  | Language code. |
| data.usage     | string  | Usage type (always "skill"). |
| data.is_default| boolean | Whether this is a default skill. |
| data.created_at| string  | Creation timestamp. |
| data.updated_at| string  | Last update timestamp. |

## Estados HTTP

- 201: Creado
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
    "content": ["The content field is required."]
  }
}
```

## Notas

- The `usage` field is automatically set to "skill".
- The authenticated user becomes the creator of the skill.
- Duplicate skill names in the same language may be prevented by validation.

## Relacionados

- [Skills — Índice](SkillÍndice.md)
- [Skills — Actualizar](SkillActualizar.md)
- [Skills — Eliminar](SkillEliminar.md)
