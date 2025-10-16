# Learn – Mostrar Contenido del Evento

## Endpoint

```
GET /api/v1/learn/events/{event}/contents/{index}
```

## Autenticación

Ninguna

## Encabezados

| Encabezado      | Tipo   | Requerido | Descripción |
| --------------- | ------ | --------- | ----------- |
| Authorization   | string | No        | `Bearer {token}` (opcional). |
| X-PUBLIC-KEY    | string | Sí        | Clave pública de la plataforma. |
| Accept-Language | string | No        | Locale IETF (ej.: `pt-BR`, `en`, `es`). |

## Parámetros

### Parámetros de ruta

| Parámetro | Tipo    | Requerido | Descripción |
| --------- | ------- | --------- | ----------- |
| event     | string  | Sí        | Identificador de recurso del evento |
| index     | integer | Sí        | Número de índice del contenido |

### Parámetros de consulta

| Parámetro | Tipo   | Requerido | Descripción | Predeterminado/Valores |
| --------- | ------ | --------- | ----------- | ---------------------- |
| language  | string | No        | Código de idioma para contenido localizado | Predeterminado de la plataforma |

> El endpoint acepta variaciones de nombres de parámetros (`camelCase`, `kebab-case`, `CapitalCase`).

## Ejemplos

### Ejemplo de solicitud (curl)

```bash
curl -X GET \
  -H "X-PUBLIC-KEY: <clave>" \
  -H "Accept-Language: es" \
  "https://sandbox.su-dominio.com/api/v1/learn/events/nombre-recurso-evento/contents/1?language=es"
```

### Ejemplo de respuesta

```json
{
  "data": {
    "uuid": "cc0e8400-e29b-41d4-a716-446655440007",
    "index_number": 1,
    "content_type": "video",
    "duration_minutes": 45,
    "is_preview": false,
    "event_title": {
      "language": "es",
      "value": "Curso Completo de Desarrollo Web"
    },
    "title": {
      "language": "es",
      "value": "Introducción al Desarrollo Web",
      "is_default": false
    },
    "description": {
      "language": "es",
      "value": "Aprende los fundamentos del desarrollo web incluyendo HTML, CSS y conceptos básicos de JavaScript."
    },
    "media": {
      "type": "video",
      "url": "https://example.com/video.mp4",
      "thumbnail": "https://example.com/thumbnail.jpg",
      "duration_seconds": 2700
    },
    "attachments": [
      {
        "name": "Material del Curso.pdf",
        "url": "https://example.com/material.pdf",
        "size_bytes": 1048576
      }
    ],
    "sub_contents_count": 3,
    "completed": false,
    "progress_percent": 0
  }
}
```

## Estructura JSON Explicada

| Campo                     | Tipo    | Descripción |
| ------------------------- | ------- | ----------- |
| data                      | object  | Detalles del contenido |
| data.uuid                 | string  | Identificador único del contenido |
| data.index_number         | integer | Índice secuencial del contenido |
| data.content_type         | string  | Tipo de contenido |
| data.duration_minutes     | integer | Duración estimada en minutos |
| data.is_preview           | boolean | Si este es un contenido de vista previa |
| data.event_title          | object  | Título localizado del evento padre |
| data.title                | object  | Título localizado del contenido |
| data.description          | object  | Descripción localizada del contenido |
| data.media                | object  | Información de medios |
| data.attachments[]        | array   | Archivos adjuntos descargables |
| data.sub_contents_count   | integer | Número de sub-contenidos |
| data.completed            | boolean | Estado de finalización del usuario (cuando autenticado) |
| data.progress_percent     | integer | Porcentaje de progreso del usuario (cuando autenticado) |

## Estados HTTP

- 200: OK
- 400: Solicitud inválida
- 401: No autorizado
- 403: Prohibido
- 404: No encontrado - Evento o contenido no encontrado
- 429: Demasiadas solicitudes
- 500: Error interno del servidor

## Errores

```json
{
  "message": "Contenido no encontrado"
}
```

## Notas

- Retorna información detallada para un contenido específico del evento por su número de índice
- El contenido se identifica por su `index_number` dentro del evento
- Incluye el título del evento padre para contexto
- Los títulos y descripciones de contenido están localizados según el parámetro `language` o encabezado `Accept-Language`
- Si el contenido localizado no está disponible, se retornan valores predeterminados
- Cuando el usuario está autenticado, incluye estado de finalización e información de progreso
- Los contenidos de vista previa son accesibles sin autenticación

## Relacionados

- [Listar Contenidos del Evento](./EventContentIndex.md)
- [Listar Sub-Contenidos del Evento](./EventContentSubContentsIndex.md)
- [Mostrar Evento](./EventShow.md)

## Changelog

- 2025-10-16: Documentación inicial
