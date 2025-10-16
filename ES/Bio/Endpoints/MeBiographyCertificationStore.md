# Bio – Mis Certificaciones Crear

## Endpoint

```
POST /api/v1/bio/me/certifications
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
{"title": "AWS Certified", "issued_date": "2024-01-15", "issuer": "Amazon"}
```

> Los parámetros aceptan camelCase, snake_case, kebab-case, or CapitalCase variants.

## Ejemplos

### Ejemplo de solicitud (curl)

```bash
curl -X POST \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Content-Type: application/json" \
  -d '{"title": "AWS Certified", "issued_date": "2024-01-15", "issuer": "Amazon"}' \
  "https://sandbox.your-domain.com/api/v1/bio/me/certifications"
```

### Ejemplo de respuesta

```json
{
  "data": {"uuid": "550e8400-e29b-41d4-a716-446655440000", "title": "AWS Certified"}
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
    "field": ["Validation error message."]
  }
}
```

## Notas

- Creates a new resource for the authenticated user.

## Relacionados

- [Mis Certificaciones — Índice](MyCertificationsÍndice.md)
- [Mis Certificaciones — Mostrar](MyCertificationsMostrar.md)
- [Mis Certificaciones — Actualizar](MyCertificationsActualizar.md)
