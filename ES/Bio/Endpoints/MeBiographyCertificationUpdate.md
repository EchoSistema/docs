# Bio – Mis Certificaciones Actualizar

## Endpoint

```
PUT /api/v1/bio/me/certifications/{certification}
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

### Parámetros de ruta

| Parámetro | Tipo   | Requerido | Descripción |
| --------- | ------ | -------- | ----------- |
| certification | string | Yes      | UUID del recurso. |

### Cuerpo de la solicitud

```json
{"title": "AWS Certified Solutions Architect"}
```

> Los parámetros aceptan camelCase, snake_case, kebab-case, or CapitalCase variants.

## Ejemplos

### Ejemplo de solicitud (curl)

```bash
curl -X PUT \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Content-Type: application/json" \
  -d '{"title": "AWS Certified Solutions Architect"}' \
  "https://sandbox.your-domain.com/api/v1/bio/me/certifications/550e8400-e29b-41d4-a716-446655440000"
```

### Ejemplo de respuesta

```json
{
  "data": {
    "uuid": "550e8400-e29b-41d4-a716-446655440000",
    "updated_at": "2025-01-15T12:00:00.000000Z"
  }
}
```

## Estados HTTP

- 200: OK
- 401: No autorizado
- 403: Prohibido
- 404: No encontrado
- 422: Entidad no procesable
- 500: Error interno del servidor

## Errores

```json
{
  "message": "You are not authorized to update this resource."
}
```

## Notas

- Only the owner can update the resource.
- All fields in the request body are optional.

## Relacionados

- [Mis Certificaciones — Índice](MyCertificationsÍndice.md)
- [Mis Certificaciones — Mostrar](MyCertificationsMostrar.md)
- [Mis Certificaciones — Eliminar](MyCertificationsEliminar.md)
