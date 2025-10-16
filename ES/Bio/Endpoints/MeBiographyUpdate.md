# Bio – Mi Biografía Actualizar

## Endpoint

```
PUT /api/v1/bio/me
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
  "slug": "john-doe-developer",
  "public_email": "john.public@example.com",
  "summary": "Experienced software engineer specializing in web development",
  "headline": "Senior Full-Stack Engineer"
}
```

| Field        | Tipo   | Requerido | Descripción |
| ------------ | ------ | -------- | ----------- |
| slug         | string | No       | URL-friendly slug. |
| public_email | string | No       | Public email address. |
| summary      | string | No       | Biography summary. |
| headline     | string | No       | Professional headline. |

> Los parámetros aceptan camelCase, snake_case, kebab-case, or CapitalCase variants.

## Ejemplos

### Ejemplo de solicitud (curl)

```bash
curl -X PUT \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Content-Type: application/json" \
  -d '{
    "slug": "john-doe-developer",
    "summary": "Experienced software engineer"
  }' \
  "https://sandbox.your-domain.com/api/v1/bio/me"
```

### Ejemplo de respuesta

```json
{
  "data": {
    "id": 1,
    "uuid": "550e8400-e29b-41d4-a716-446655440000",
    "user_id": 123,
    "slug": "john-doe-developer",
    "public_email": "john.public@example.com",
    "summary": "Experienced software engineer",
    "headline": "Senior Full-Stack Engineer",
    "updated_at": "2025-01-15T12:00:00.000000Z"
  }
}
```

## Estados HTTP

- 200: OK
- 401: No autorizado
- 403: Prohibido
- 422: Entidad no procesable
- 500: Error interno del servidor

## Errores

```json
{
  "message": "The given data was invalid.",
  "errors": {
    "slug": ["The slug has already been taken."]
  }
}
```

## Notas

- All fields are optional in the update request.
- The slug is automatically generated from the provided value.
- Actualizars only the authenticated user's biography.

## Relacionados

- [Mi Biografía — Mostrar](MeBiographyMostrar.md)
