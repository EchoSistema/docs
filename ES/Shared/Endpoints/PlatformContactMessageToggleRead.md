# Shared – Alternar Estado de Lectura de Mensaje

## Endpoint

`PATCH /api/v1/contacts/{message:uuid}/toggle-read`

## Autenticación

Requiere token Bearer y cabecera de plataforma.

## Encabezados

| Encabezado | Tipo | Requerido | Descripción |
| ---------- | ---- | --------- | ----------- |
| Authorization | `string` | Sí | `Bearer {token}`. |
| X-PUBLIC-KEY  | `string` | Sí | Clave pública de la plataforma. |
| Accept-Language | `string` | No | Locale para respuestas. |

## Parámetros

Alternar el estado de lectura de un mensaje de contacto. Permisos: `show.platform_message` o `show.all`, excepto cuando el usuario autenticado es el autor. Usuarios no backoffice solo pueden alternar mensajes de su plataforma.

## Ejemplos

### Ejemplo de solicitud (curl)

```bash
curl -X PATCH \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: es" \
  "https://sandbox.your-domain.com/api/v1/contacts/{message:uuid}/toggle-read"
```

### Éxito `200 OK`

```json
{
  "data": {
    "uuid": "2c3f9d26-4c68-4478-8d1f-9a1ea3b6b32e",
    "name": "Maria Silva",
    "is_read": true,
    "email": "maria.silva@example.com",
    "created_at": "2024-06-06T18:12:45+00:00"
  }
}
```

## Estructura JSON Explicada

| Campo | Tipo | Descripción |
| ----- | ---- | ----------- |
| data  | `object` | Recurso del mensaje (igual al endpoint de detalle). |

## Estados HTTP

- 200: OK
- 400: Solicitud inválida
- 401: No autorizado
- 403: Prohibido
- 404: No encontrado
- 422: Entidad no procesable
- 429: Demasiadas solicitudes
- 500: Error interno del servidor

## Errores

```json
{
  "message": "Mensaje de error"
}
```

## Notas

- Alternar el estado de lectura de un mensaje de contacto

## Relacionados

- See other Shared API endpoints

## Changelog

- 2025-10-16: Documentación inicial
