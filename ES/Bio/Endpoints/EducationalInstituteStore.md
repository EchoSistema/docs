# Bio – Educational Institutes Crear

## Endpoint

```
POST /api/v1/bio/educational-institutes
```

## Autenticación

Requerida – Bearer {token} con habilidad `backoffice` y permiso `domain:bio`.

## Encabezados

| Encabezado           | Tipo   | Requerido | Descripción |
| ---------------- | ------ | -------- | ----------- |
| Authorization    | string | Yes      | `Bearer {token}`. |
| X-PUBLIC-KEY     | string | Yes      | Clave pública de la plataforma. |
| Content-Type     | string | Yes      | `application/json`. |

## Parámetros

### Cuerpo de la solicitud

```json
{
  "name": "MIT",
  "country": "US"
}
```

| Field   | Tipo   | Requerido | Descripción |
| ------- | ------ | -------- | ----------- |
| name    | string | Yes      | Institute name. |
| country | string | Yes      | Country code (ISO 2-letter). |

> Los parámetros aceptan camelCase, snake_case, kebab-case, or CapitalCase variants.

## Ejemplos

### Ejemplo de solicitud (curl)

```bash
curl -X POST \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Content-Type: application/json" \
  -d '{"name": "MIT", "country": "US"}' \
  "https://sandbox.your-domain.com/api/v1/bio/educational-institutes"
```

### Ejemplo de respuesta

```json
{
  "data": {
    "id": 1,
    "uuid": "550e8400-e29b-41d4-a716-446655440000",
    "name": "MIT",
    "country": "US",
    "created_at": "2025-01-15T10:30:00.000000Z",
    "updated_at": "2025-01-15T10:30:00.000000Z"
  }
}
```

## Estados HTTP

- 201: Creado
- 400: Solicitud inválida
- 401: No autorizado
- 422: Entidad no procesable
- 500: Error interno del servidor

## Errores

```json
{
  "message": "The given data was invalid.",
  "errors": {
    "name": ["The name field is required."]
  }
}
```

## Notas

- The authenticated user becomes the creator.

## Relacionados

- [Educational Institutes — Índice](EducationalInstituteÍndice.md)
- [Educational Institutes — Actualizar](EducationalInstituteActualizar.md)
