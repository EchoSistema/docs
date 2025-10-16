# Bio – Educational Institutes Actualizar

## Endpoint

```
PUT /api/v1/bio/educational-institutes/{uuid}
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
| uuid      | string | Yes      | Educational institute UUID. |

### Cuerpo de la solicitud

```json
{
  "name": "Massachusetts Institute of Technology"
}
```

| Field   | Tipo   | Requerido | Descripción |
| ------- | ------ | -------- | ----------- |
| name    | string | No       | Institute name. |
| country | string | No       | Country code (ISO 2-letter). |

## Ejemplos

### Ejemplo de solicitud (curl)

```bash
curl -X PUT \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Content-Type: application/json" \
  -d '{"name": "Massachusetts Institute of Technology"}' \
  "https://sandbox.your-domain.com/api/v1/bio/educational-institutes/550e8400-e29b-41d4-a716-446655440000"
```

### Ejemplo de respuesta

```json
{
  "data": {
    "id": 1,
    "uuid": "550e8400-e29b-41d4-a716-446655440000",
    "name": "Massachusetts Institute of Technology",
    "country": "US",
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
  "message": "You are not authorized to update this educational institute."
}
```

## Notas

- Only the creator can update the educational institute.

## Relacionados

- [Educational Institutes — Índice](EducationalInstituteÍndice.md)
- [Educational Institutes — Eliminar](EducationalInstituteEliminar.md)
