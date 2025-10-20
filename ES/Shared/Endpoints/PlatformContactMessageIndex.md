# Shared – Listar Mensajes de Contacto

## Endpoint

`GET /api/v1/contacts`

## Autenticación

Requiere token Bearer y cabecera de plataforma.

## Encabezados

| Encabezado | Tipo | Requerido | Descripción |
| ---------- | ---- | --------- | ----------- |
| Authorization | `string` | Sí | `Bearer {token}`. |
| X-PUBLIC-KEY  | `string` | Sí | Clave pública de la plataforma. |
| Accept-Language | `string` | No | Locale para respuestas. |

## Parámetros

Listar mensajes de contacto con filtros y paginación. Permisos: `index.platform_message` o `index.all`. Usuarios no backoffice se limitan a su plataforma; invitados ven solo sus envíos.

### Parámetros de consulta

| Parámetro | Tipo | Descripción |
| --------- | ---- | ----------- |
| `page` | `integer` | Número de página (por defecto 1). |
| `per_page` | `integer` | Ítems por página (por defecto 25). |
| `no_paginate` | `boolean` | Desactiva la paginación. |
| `limit` | `integer` | Límite cuando `no_paginate` es verdadero. |
| `sort_by` | `string` | Columna de orden (por defecto `created_at`). |
| `direction` | `string` | `ASC`/`DESC` (por defecto `DESC`). |
| `uuid` | `uuid` | Identificador exacto. |
| `email` | `email` | Correo del contacto. |
| `phone` | `string` | Teléfono informado. |
| `name` | `string` | Nombre (búsqueda parcial). |
| `type` | `string` | `default`, `denunciation`, `report`. |
| `message_lang` | `string` | `pt-BR`, `en`, `es`, `gn`. |
| `created_at` | `date` | Fecha exacta (`YYYY-MM-DD`). |
| `interval` | `string` | `hour`, `day`, `week`, `month`, `year`. |
| `period.from` / `period.to` | `date` | Rango de fechas personalizado. |
| `user` | `string` | ID, UUID, correo, o parte del nombre del autor. |
| `platform` | `string` | ID, UUID, slug, o parte del nombre de la plataforma. |
| `ip_address` | `ipv4` | IP del remitente. |
| `ip_banned` | `boolean` | Solo IPs bloqueadas. |
| `is_robot` | `boolean` | Solo IPs identificadas como robots. |
| `is_read` | `boolean` | Filtra por estado de lectura. |
| `first_reader` | `string` | Identificador del primer lector. |
| `content` | `string` | Búsqueda de texto en el cuerpo. |
| `with_images` | `boolean` | Solo mensajes con adjuntos. |

## Ejemplos

### Ejemplo de solicitud (curl)

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: es" \
  "https://sandbox.your-domain.com/api/v1/contacts"
```

### Ejemplo de respuesta (200 OK)

```json
{
  "data": [
    {
      "uuid": "2c3f9d26-4c68-4478-8d1f-9a1ea3b6b32e",
      "name": "Maria Silva",
      "is_read": false,
      "email": "maria.silva@example.com",
      "created_at": "2024-06-06T18:12:45+00:00",
      "user": { "uuid": "1fdf0b1e-2cde-4b12-93f5-02d41a0f38c4", "name": "Maria Silva", "email": "maria.silva@example.com" },
      "content": "Me gustaría saber más sobre los planes..."
    }
  ],
  "links": { "first": "https://api.example.com/api/v1/contacts?page=1", "last": "https://api.example.com/api/v1/contacts?page=5", "prev": null, "next": "https://api.example.com/api/v1/contacts?page=2" },
  "meta": { "current_page": 1, "from": 1, "last_page": 5, "path": "https://api.example.com/api/v1/contacts", "per_page": 25, "to": 25, "total": 112 }
}
```

## Estructura JSON Explicada

| Campo | Tipo | Descripción |
| ----- | ---- | ----------- |
| data  | `array` | Lista de mensajes. Metadatos de paginación cuando aplica. |

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

- Listar todos los mensajes de contacto (con filtros)

## Relacionados

- See other Shared API endpoints

## Changelog

- 2025-10-16: Documentación inicial
