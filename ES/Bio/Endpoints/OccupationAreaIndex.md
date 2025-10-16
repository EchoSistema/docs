# Bio – Occupation Areas Índice

## Endpoint

```
GET /api/v1/bio/occupation-areas
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

| Parámetro  | Tipo    | Requerido | Descripción | Predeterminado/Valores |
| ---------- | ------- | -------- | ----------- | -------------- |
| noPaginate | boolean | No       | Devolver todos los registros sin paginación. | false |
| perPage    | integer | No       | Número de elementos por página. | 50 |

> Los parámetros aceptan camelCase, snake_case, kebab-case, or CapitalCase variants.

## Ejemplos

### Ejemplo de solicitud (curl)

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: en" \
  "https://sandbox.your-domain.com/api/v1/bio/occupation-areas"
```

### Ejemplo de respuesta

```json
{
  "data": [
    {
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
  ],
  "links": {
    "first": "https://sandbox.your-domain.com/api/v1/bio/occupation-areas?page=1",
    "last": "https://sandbox.your-domain.com/api/v1/bio/occupation-areas?page=5",
    "prev": null,
    "next": "https://sandbox.your-domain.com/api/v1/bio/occupation-areas?page=2"
  },
  "meta": {
    "current_page": 1,
    "from": 1,
    "last_page": 5,
    "per_page": 50,
    "to": 50,
    "total": 250
  }
}
```

## JSON Structure Explained

| Field                  | Tipo    | Descripción |
| ---------------------- | ------- | ----------- |
| data[]                 | array   | List of occupation areas. |
| data[].id              | integer | Occupation area identifier. |
| data[].uuid            | string  | Unique occupation area identifier. |
| data[].titles[]        | array   | Multi-language titles. |
| data[].titles[].id     | integer | Title identifier. |
| data[].titles[].content| string  | Title in specific language. |
| data[].titles[].language| string | Language code. |
| data[].created_at      | string  | Creation timestamp. |
| data[].updated_at      | string  | Last update timestamp. |
| links                  | object  | Pagination links (when paginated). |
| meta                   | object  | Pagination metadata (when paginated). |

## Estados HTTP

- 200: OK
- 401: No autorizado
- 403: Prohibido
- 429: Demasiadas solicitudes
- 500: Error interno del servidor

## Errores

Standard error responses:

```json
{
  "message": "Unauthenticated."
}
```

## Notas

- Occupation areas are returned with all available language translations.
- Use `noPaginate=true` to retrieve all records at once.
- Default pagination is 50 items per page.

## Relacionados

- [Occupation Areas — Crear](OccupationAreaCrear.md)
- [Occupation Areas — Actualizar](OccupationAreaActualizar.md)
- [Occupation Areas — Eliminar](OccupationAreaEliminar.md)
