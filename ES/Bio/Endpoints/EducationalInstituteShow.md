# Bio – Educational Institutes Mostrar

## Endpoint

```
GET /api/v1/bio/educational-institutes/{uuid}
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
| uuid      | string | Yes      | Educational institute UUID. |

## Ejemplos

### Ejemplo de solicitud (curl)

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  "https://sandbox.your-domain.com/api/v1/bio/educational-institutes/550e8400-e29b-41d4-a716-446655440000"
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

## JSON Structure Explained

| Field          | Tipo    | Descripción |
| -------------- | ------- | ----------- |
| data           | object  | Educational institute object. |
| data.id        | integer | Institute identifier. |
| data.uuid      | string  | Unique institute identifier. |
| data.name      | string  | Institute name. |
| data.country   | string  | Country code. |
| data.created_at| string  | Creation timestamp. |
| data.updated_at| string  | Last update timestamp. |

## Estados HTTP

- 200: OK
- 401: No autorizado
- 403: Prohibido
- 404: No encontrado
- 429: Demasiadas solicitudes
- 500: Error interno del servidor

## Errores

```json
{
  "message": "Educational institute not found."
}
```

## Notas

- Any authenticated user can view any educational institute.

## Relacionados

- [Educational Institutes — Índice](EducationalInstituteÍndice.md)
- [Educational Institutes — Actualizar](EducationalInstituteActualizar.md)
