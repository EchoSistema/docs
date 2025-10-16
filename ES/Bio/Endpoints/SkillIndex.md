# Bio – Skills Índice

## Endpoint

```
GET /api/v1/bio/skills
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

### Parámetros de consulta

| Parámetro       | Tipo    | Requerido | Descripción | Predeterminado/Valores |
| --------------- | ------- | -------- | ----------- | -------------- |
| noPaginate      | boolean | No       | Devolver todos los registros sin paginación. | false |
| perPage         | integer | No       | Número de elementos por página. | 50 |
| skill_language  | string  | No       | Filter skills by language code (e.g., `en`, `pt-BR`). | Uses default language |

> Document the canonical parameter names in `snake_case`. When applicable, mention that variants are accepted (`camelCase`, `kebab-case`, `CapitalCase`).

## Ejemplos

### Ejemplo de solicitud (curl)

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: en" \
  "https://sandbox.your-domain.com/api/v1/bio/skills?perPage=25"
```

### Ejemplo de respuesta

```json
{
  "data": [
    {
      "id": 1,
      "uuid": "550e8400-e29b-41d4-a716-446655440000",
      "content": "JavaScript",
      "language": "en",
      "usage": "skill",
      "is_default": true,
      "created_at": "2025-01-15T10:30:00.000000Z",
      "updated_at": "2025-01-15T10:30:00.000000Z"
    }
  ],
  "links": {
    "first": "https://sandbox.your-domain.com/api/v1/bio/skills?page=1",
    "last": "https://sandbox.your-domain.com/api/v1/bio/skills?page=10",
    "prev": null,
    "next": "https://sandbox.your-domain.com/api/v1/bio/skills?page=2"
  },
  "meta": {
    "current_page": 1,
    "from": 1,
    "last_page": 10,
    "per_page": 50,
    "to": 50,
    "total": 500
  }
}
```

## JSON Structure Explained

| Field                | Tipo    | Descripción |
| -------------------- | ------- | ----------- |
| data[]               | array   | List of skills. |
| data[].id            | integer | Skill identifier. |
| data[].uuid          | string  | Unique skill identifier. |
| data[].content       | string  | Skill name/title. |
| data[].language      | string  | Language code. |
| data[].usage         | string  | Usage type (always "skill"). |
| data[].is_default    | boolean | Whether this is a default skill. |
| data[].created_at    | string  | Creation timestamp. |
| data[].updated_at    | string  | Last update timestamp. |
| links                | object  | Pagination links (when paginated). |
| meta                 | object  | Pagination metadata (when paginated). |

## Estados HTTP

- 200: OK
- 401: No autorizado
- 403: Prohibido
- 429: Demasiadas solicitudes
- 500: Error interno del servidor

## Errores

Standard error responses follow this format:

```json
{
  "message": "Unauthenticated."
}
```

## Notas

- By default, the endpoint returns skills filtered by `is_default = true` and default platform language.
- Use `skill_language` parameter to filter by specific language.
- Use `noPaginate=true` to retrieve all records at once (use with caution for large datasets).
- Default pagination is 50 items per page.

## Relacionados

- [Skills — Crear](SkillCrear.md)
- [Skills — Actualizar](SkillActualizar.md)
- [Skills — Eliminar](SkillEliminar.md)
- [Skills — Batch Eliminar](SkillBatchEliminar.md)
