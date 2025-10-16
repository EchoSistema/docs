# Bio – Mis Certificaciones Mostrar

## Endpoint

```
GET /api/v1/bio/me/certifications/{certification}
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

### Parámetros de ruta

| Parámetro | Tipo   | Requerido | Descripción |
| --------- | ------ | -------- | ----------- |
| certification | string | Yes      | UUID del recurso. |

## Ejemplos

### Ejemplo de solicitud (curl)

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  "https://sandbox.your-domain.com/api/v1/bio/me/certifications/550e8400-e29b-41d4-a716-446655440000"
```

### Ejemplo de respuesta

```json
{
  "data": {"uuid": "550e8400-e29b-41d4-a716-446655440000", "title": "AWS Certified", "issued_date": "2024-01-15"}
}
```

## Estados HTTP

- 200: OK
- 401: No autorizado
- 403: Prohibido
- 404: No encontrado
- 500: Error interno del servidor

## Errores

```json
{
  "message": "Resource not found."
}
```

## Notas

- Only returns items owned by the authenticated user.

## Relacionados

- [Mis Certificaciones — Índice](MyCertificationsÍndice.md)
- [Mis Certificaciones — Actualizar](MyCertificationsActualizar.md)
- [Mis Certificaciones — Eliminar](MyCertificationsEliminar.md)
