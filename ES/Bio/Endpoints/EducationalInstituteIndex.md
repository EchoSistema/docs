# Bio – Educational Institutes Índice

## Endpoint

```
GET /api/v1/bio/educational-institutes
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
| perPage    | integer | No       | Número de elementos por página. | 25 |

> Los parámetros aceptan camelCase, snake_case, kebab-case, or CapitalCase variants.

## Ejemplos

### Ejemplo de solicitud (curl)

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: en" \
  "https://sandbox.your-domain.com/api/v1/bio/educational-institutes"
```

### Ejemplo de respuesta

```json
{
  "data": [
    {
      "id": 1,
      "uuid": "550e8400-e29b-41d4-a716-446655440000",
      "name": "MIT",
      "country": "US",
      "created_at": "2025-01-15T10:30:00.000000Z",
      "updated_at": "2025-01-15T10:30:00.000000Z"
    }
  ],
  "links": {
    "first": "https://sandbox.your-domain.com/api/v1/bio/educational-institutes?page=1",
    "last": "https://sandbox.your-domain.com/api/v1/bio/educational-institutes?page=5",
    "prev": null,
    "next": "https://sandbox.your-domain.com/api/v1/bio/educational-institutes?page=2"
  },
  "meta": {
    "current_page": 1,
    "per_page": 25,
    "total": 125
  }
}
```

## JSON Structure Explained

| Field             | Tipo    | Descripción |
| ----------------- | ------- | ----------- |
| data[]            | array   | List of educational institutes. |
| data[].id         | integer | Institute identifier. |
| data[].uuid       | string  | Unique institute identifier. |
| data[].name       | string  | Institute name. |
| data[].country    | string  | Country code. |
| data[].created_at | string  | Creation timestamp. |
| data[].updated_at | string  | Last update timestamp. |

## Estados HTTP

- 200: OK
- 401: No autorizado
- 403: Prohibido
- 429: Demasiadas solicitudes
- 500: Error interno del servidor

## Errores

```json
{
  "message": "Unauthenticated."
}
```

## Notas

- Returns all educational institutes regardless of creator.
- Default pagination is 25 items per page.

## Relacionados

- [Educational Institutes — Mostrar](EducationalInstituteMostrar.md)
- [Educational Institutes — Crear](EducationalInstituteCrear.md)
