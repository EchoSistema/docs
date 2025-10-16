# Bio – Mis Certificaciones Eliminar

## Endpoint

```
DELETE /api/v1/bio/me/certifications/{certification}
```

## Autenticación

Requerida – Bearer {token} con habilidad `backoffice` y permiso `domain:bio`.

## Encabezados

| Encabezado           | Tipo   | Requerido | Descripción |
| ---------------- | ------ | -------- | ----------- |
| Authorization    | string | Yes      | `Bearer {token}`. |
| X-PUBLIC-KEY     | string | Yes      | Clave pública de la plataforma. |

## Parámetros

### Parámetros de ruta

| Parámetro | Tipo   | Requerido | Descripción |
| --------- | ------ | -------- | ----------- |
| certification | string | Yes      | UUID del recurso. |

## Ejemplos

### Ejemplo de solicitud (curl)

```bash
curl -X DELETE \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  "https://sandbox.your-domain.com/api/v1/bio/me/certifications/550e8400-e29b-41d4-a716-446655440000"
```

### Ejemplo de respuesta

```json
{
  "success": true,
  "message": "Certification deleted successfully."
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
  "message": "You are not authorized to delete this resource."
}
```

## Notas

- Only the owner can delete the resource.
- Deletion is permanent and cannot be undone.

## Relacionados

- [Mis Certificaciones — Índice](MyCertificationsÍndice.md)
- [Mis Certificaciones — Batch Eliminar](MyCertificationsBatchEliminar.md)
