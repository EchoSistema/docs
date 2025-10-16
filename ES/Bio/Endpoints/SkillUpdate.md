# Bio – Skills Actualizar

## Endpoint

```
PUT /api/v1/bio/skills/{uuid}
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
| uuid      | string | Yes      | Skill UUID. |

### Cuerpo de la solicitud

```json
{
  "content": "Python 3.x",
  "language": "en",
  "is_default": true
}
```

| Field      | Tipo    | Requerido | Descripción |
| ---------- | ------- | -------- | ----------- |
| content    | string  | No       | Skill name/title. |
| language   | string  | No       | Language code (e.g., `en`, `pt-BR`). |
| is_default | boolean | No       | Whether this is a default skill. |

> Los parámetros aceptan camelCase, snake_case, kebab-case, or CapitalCase variants.

## Ejemplos

### Ejemplo de solicitud (curl)

```bash
curl -X PUT \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: en" \
  -H "Content-Type: application/json" \
  -d '{
    "content": "Python 3.x",
    "is_default": true
  }' \
  "https://sandbox.your-domain.com/api/v1/bio/skills/550e8400-e29b-41d4-a716-446655440000"
```

### Ejemplo de respuesta

```json
{
  "data": {
    "id": 123,
    "uuid": "550e8400-e29b-41d4-a716-446655440000",
    "content": "Python 3.x",
    "language": "en",
    "usage": "skill",
    "is_default": true,
    "created_at": "2025-01-15T10:30:00.000000Z",
    "updated_at": "2025-01-15T11:45:00.000000Z"
  }
}
```

## JSON Structure Explained

| Field          | Tipo    | Descripción |
| -------------- | ------- | ----------- |
| data           | object  | Actualizard skill object. |
| data.id        | integer | Skill identifier. |
| data.uuid      | string  | Unique skill identifier. |
| data.content   | string  | Skill name/title. |
| data.language  | string  | Language code. |
| data.usage     | string  | Usage type (always "skill"). |
| data.is_default| boolean | Whether this is a default skill. |
| data.created_at| string  | Creation timestamp. |
| data.updated_at| string  | Last update timestamp. |

## Estados HTTP

- 200: OK
- 400: Solicitud inválida
- 401: No autorizado
- 403: Prohibido
- 404: No encontrado
- 422: Entidad no procesable
- 429: Demasiadas solicitudes
- 500: Error interno del servidor

## Errores

Not found error example:

```json
{
  "message": "Skill not found."
}
```

Forbidden error example (not creator):

```json
{
  "message": "You are not authorized to update this skill."
}
```

## Notas

- Only the creator of the skill can update it.
- All fields are optional in the update request.
- The `updated_at` timestamp is automatically updated.

## Relacionados

- [Skills — Índice](SkillÍndice.md)
- [Skills — Crear](SkillCrear.md)
- [Skills — Eliminar](SkillEliminar.md)
