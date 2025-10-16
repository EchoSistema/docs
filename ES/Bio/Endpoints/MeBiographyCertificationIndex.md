# Bio – Mis Certificaciones Índice

## Endpoint

```
GET /api/v1/bio/me/certifications
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
  "https://sandbox.your-domain.com/api/v1/bio/me/certifications"
```

### Ejemplo de respuesta

```json
{
  "data": [
    {"uuid": "550e8400-e29b-41d4-a716-446655440000", "title": "AWS Certified", "issued_date": "2024-01-15"}
  ],
  "links": {
    "first": "https://sandbox.your-domain.com/api/v1/bio/me/certifications?page=1",
    "last": "https://sandbox.your-domain.com/api/v1/bio/me/certifications?page=5",
    "prev": null,
    "next": "https://sandbox.your-domain.com/api/v1/bio/me/certifications?page=2"
  },
  "meta": {
    "current_page": 1,
    "per_page": 25,
    "total": 100
  }
}
```

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

- Returns certifications for the authenticated user only.
- Use `noPaginate=true` to retrieve all records at once.
- Default pagination is 25 items per page.

## Relacionados

- [Mis Certificaciones — Mostrar](MyCertificationsMostrar.md)
- [Mis Certificaciones — Crear](MyCertificationsCrear.md)
- [Mis Certificaciones — Actualizar](MyCertificationsActualizar.md)
- [Mis Certificaciones — Eliminar](MyCertificationsEliminar.md)
