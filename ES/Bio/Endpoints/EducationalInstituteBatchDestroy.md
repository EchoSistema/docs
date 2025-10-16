# Bio – Educational Institutes Batch Eliminar

## Endpoint

```
DELETE /api/v1/bio/educational-institutes
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
  "institutes": [
    "550e8400-e29b-41d4-a716-446655440000",
    "660e8400-e29b-41d4-a716-446655440001"
  ]
}
```

| Field      | Tipo  | Requerido | Descripción |
| ---------- | ----- | -------- | ----------- |
| institutes | array | Yes      | Array of educational institute UUIDs. |

## Ejemplos

### Ejemplo de solicitud (curl)

```bash
curl -X DELETE \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Content-Type: application/json" \
  -d '{"institutes": ["550e8400-e29b-41d4-a716-446655440000"]}' \
  "https://sandbox.your-domain.com/api/v1/bio/educational-institutes"
```

### Ejemplo de respuesta

```json
{
  "success": true,
  "message": "2 educational institutes deleted successfully."
}
```

## Estados HTTP

- 200: OK
- 401: No autorizado
- 422: Entidad no procesable
- 500: Error interno del servidor

## Errores

```json
{
  "message": "The given data was invalid.",
  "errors": {
    "institutes": ["The institutes field is required."]
  }
}
```

## Notas

- Only items created by the authenticated user can be deleted.
- Items not found or not owned are silently skipped.

## Relacionados

- [Educational Institutes — Índice](EducationalInstituteÍndice.md)
- [Educational Institutes — Eliminar](EducationalInstituteEliminar.md)
