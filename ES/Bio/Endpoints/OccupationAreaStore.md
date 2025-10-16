# Bio – Occupation Areas Crear

## Endpoint

```
POST /api/v1/bio/occupation-areas
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
  "titles": [
    {
      "content": "Software Development",
      "language": "en"
    },
    {
      "content": "Desenvolvimento de Software",
      "language": "pt-BR"
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
curl -X POST \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: en" \
  -H "Content-Type: application/json" \
  -d '{
    "titles": [
      {"content": "Software Development", "language": "en"},
      {"content": "Desenvolvimento de Software", "language": "pt-BR"}
    ]
  }' \
  "https://sandbox.your-domain.com/api/v1/bio/occupation-areas"
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
        "content": "Software Development",
        "language": "en"
      },
      {
        "id": 11,
        "content": "Desenvolvimento de Software",
        "language": "pt-BR"
      }
    ],
    "created_at": "2025-01-15T10:30:00.000000Z",
    "updated_at": "2025-01-15T10:30:00.000000Z"
  }
}
```

## JSON Structure Explained

| Field                  | Tipo    | Descripción |
| ---------------------- | ------- | ----------- |
| data                   | object  | Created occupation area. |
| data.id                | integer | Occupation area identifier. |
| data.uuid              | string  | Unique occupation area identifier. |
| data.titles[]          | array   | Multi-language titles. |
| data.titles[].id       | integer | Title identifier. |
| data.titles[].content  | string  | Title in specific language. |
| data.titles[].language | string  | Language code. |
| data.created_at        | string  | Creation timestamp. |
| data.updated_at        | string  | Last update timestamp. |

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
    "titles": ["The titles field is required."]
  }
}
```

## Notas

- At least one title is required.
- Multiple language translations can be provided in a single request.
- The authenticated user becomes the creator.

## Relacionados

- [Occupation Areas — Índice](OccupationAreaÍndice.md)
- [Occupation Areas — Actualizar](OccupationAreaActualizar.md)
