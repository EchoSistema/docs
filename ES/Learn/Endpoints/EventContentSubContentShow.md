# Learn – Mostrar Sub-Contenido del Evento

## Endpoint

```
GET /api/v1/learn/events/{event}/contents/{index}/sub-contents/{subIndex}
```

## Autenticación

Requerida – Bearer {token}

## Encabezados

| Encabezado      | Tipo   | Requerido | Descripción |
| --------------- | ------ | --------- | ----------- |
| Authorization   | string | Sí        | `Bearer {token}`. |
| X-PUBLIC-KEY    | string | Sí        | Clave pública de la plataforma. |
| Accept-Language | string | No        | Locale IETF (ej.: `pt-BR`, `en`, `es`). |

## Parámetros

### Parámetros de ruta

| Parámetro | Tipo    | Requerido | Descripción |
| --------- | ------- | --------- | ----------- |
| event     | string  | Sí        | Identificador de recurso del evento |
| index     | integer | Sí        | Número de índice del contenido padre |
| subIndex  | integer | Sí        | Número de índice del sub-contenido |

### Parámetros de consulta

| Parámetro   | Tipo   | Requerido | Descripción | Predeterminado/Valores |
| ----------- | ------ | --------- | ----------- | ---------------------- |
| language    | string | No        | Código de idioma para contenido localizado | Predeterminado de la plataforma |
| platform_id | string | No        | UUID de plataforma para validación de propiedad | Derivado de X-PUBLIC-KEY |

> El endpoint acepta variaciones de nombres de parámetros (`camelCase`, `kebab-case`, `CapitalCase`).

## Ejemplos

### Ejemplo de solicitud (curl)

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <clave>" \
  -H "Accept-Language: es" \
  "https://sandbox.su-dominio.com/api/v1/learn/events/nombre-recurso-evento/contents/1/sub-contents/1?language=es"
```

### Ejemplo de respuesta

```json
{
  "data": {
    "uuid": "ee0e8400-e29b-41d4-a716-446655440009",
    "index_number": 1,
    "parent_content": {
      "uuid": "cc0e8400-e29b-41d4-a716-446655440007",
      "index_number": 1,
      "title": {
        "language": "es",
        "value": "Introducción al Desarrollo Web"
      }
    },
    "content_type": "video",
    "duration_minutes": 15,
    "title": {
      "language": "es",
      "value": "Fundamentos de HTML - Parte 1",
      "is_default": false
    },
    "description": {
      "language": "es",
      "value": "Aprende sobre etiquetas HTML, elementos y estructura de documentos. Esta lección cubre los bloques básicos de construcción de HTML."
    },
    "media": {
      "type": "video",
      "url": "https://example.com/sub-video-1.mp4",
      "thumbnail": "https://example.com/sub-thumb-1.jpg",
      "duration_seconds": 900
    },
    "attachments": [
      {
        "name": "Guía Rápida HTML.pdf",
        "url": "https://example.com/html-cheatsheet.pdf",
        "size_bytes": 512000
      }
    ],
    "completed": false,
    "progress_percent": 0
  }
}
```

## Estructura JSON Explicada

| Campo                        | Tipo    | Descripción |
| ---------------------------- | ------- | ----------- |
| data                         | object  | Detalles del sub-contenido |
| data.uuid                    | string  | Identificador único del sub-contenido |
| data.index_number            | integer | Índice secuencial del sub-contenido |
| data.parent_content          | object  | Información del contenido padre |
| data.parent_content.index_number | integer | Índice del contenido padre |
| data.parent_content.title    | object  | Título localizado del contenido padre |
| data.content_type            | string  | Tipo de sub-contenido |
| data.duration_minutes        | integer | Duración estimada en minutos |
| data.title                   | object  | Título localizado del sub-contenido |
| data.description             | object  | Descripción localizada del sub-contenido |
| data.media                   | object  | Información de medios |
| data.attachments[]           | array   | Archivos adjuntos descargables |
| data.completed               | boolean | Estado de finalización del usuario |
| data.progress_percent        | integer | Porcentaje de progreso del usuario |

## Estados HTTP

- 200: OK
- 400: Solicitud inválida
- 401: No autorizado - Autenticación requerida
- 403: Prohibido - La plataforma no posee este evento
- 404: No encontrado - Evento, contenido o sub-contenido no encontrado
- 429: Demasiadas solicitudes
- 500: Error interno del servidor

## Errores

```json
{
  "message": "Sub-contenido no encontrado"
}
```

```json
{
  "message": "No autenticado"
}
```

## Notas

- Retorna información detallada para un sub-contenido específico por su número de índice
- Requiere autenticación ya que los sub-contenidos generalmente no son contenido de vista previa
- El evento debe pertenecer a la plataforma que realiza la solicitud
- La propiedad de la plataforma se valida usando el parámetro `platform_id` o derivado de `X-PUBLIC-KEY`
- Incluye la información del contenido padre para contexto
- Los títulos y descripciones están localizados según el parámetro `language` o encabezado `Accept-Language`
- Si el contenido localizado no está disponible, se retornan valores predeterminados
- Incluye estado de finalización e información de progreso para el usuario autenticado

## Relacionados

- [Listar Sub-Contenidos del Evento](./EventContentSubContentsIndex.md)
- [Mostrar Contenido del Evento](./EventContentShow.md)
- [Listar Contenidos del Evento](./EventContentIndex.md)

## Changelog

- 2025-10-16: Documentación inicial
