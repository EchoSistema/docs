# Learn – Listar Contenidos del Evento

## Endpoint

```
GET /api/v1/learn/events/{event}/contents
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

| Parámetro | Tipo   | Requerido | Descripción |
| --------- | ------ | --------- | ----------- |
| event     | string | Sí        | Identificador de recurso del evento |

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
  "https://sandbox.su-dominio.com/api/v1/learn/events/nombre-recurso-evento/contents?language=es"
```

### Ejemplo de respuesta

```json
{
  "data": [
    {
      "uuid": "cc0e8400-e29b-41d4-a716-446655440007",
      "index_number": 1,
      "content_type": "video",
      "duration_minutes": 45,
      "is_preview": false,
      "title": {
        "language": "es",
        "value": "Introducción al Desarrollo Web",
        "is_default": false
      },
      "description": {
        "language": "es",
        "value": "Aprende los fundamentos del desarrollo web..."
      },
      "media": {
        "url": "https://example.com/video.mp4",
        "thumbnail": "https://example.com/thumbnail.jpg"
      },
      "sub_contents_count": 3
    },
    {
      "uuid": "dd0e8400-e29b-41d4-a716-446655440008",
      "index_number": 2,
      "content_type": "text",
      "duration_minutes": 15,
      "is_preview": true,
      "title": {
        "language": "es",
        "value": "Fundamentos de HTML"
      },
      "sub_contents_count": 0
    }
  ]
}
```

## Estructura JSON Explicada

| Campo                      | Tipo    | Descripción |
| -------------------------- | ------- | ----------- |
| data[]                     | array   | Lista de contenidos del evento |
| data[].uuid                | string  | Identificador único del contenido |
| data[].index_number        | integer | Índice secuencial del contenido |
| data[].content_type        | string  | Tipo de contenido (video, text, pdf, etc.) |
| data[].duration_minutes    | integer | Duración estimada en minutos |
| data[].is_preview          | boolean | Si este contenido está disponible como vista previa |
| data[].title               | object  | Título localizado del contenido |
| data[].description         | object  | Descripción localizada del contenido |
| data[].media               | object  | URLs de medios (cuando aplica) |
| data[].sub_contents_count  | integer | Número de sub-contenidos |

## Estados HTTP

- 200: OK
- 400: Solicitud inválida
- 401: No autorizado
- 403: Prohibido
- 404: No encontrado - Evento no encontrado
- 429: Demasiadas solicitudes
- 500: Error interno del servidor

## Errores

```json
{
  "message": "Evento no encontrado"
}
```

## Notas

- Retorna todos los contenidos para un evento específico
- Los contenidos están ordenados por `index_number`
- Los títulos y descripciones de contenido están localizados según el parámetro `language` o encabezado `Accept-Language`
- Si el contenido localizado no está disponible, se retornan valores predeterminados
- El campo `sub_contents_count` indica si el contenido tiene sub-contenidos anidados
- Los contenidos de vista previa (`is_preview: true`) generalmente están disponibles sin autenticación

## Relacionados

- [Mostrar Contenido del Evento](./EventContentShow.md)
- [Listar Sub-Contenidos del Evento](./EventContentSubContentsIndex.md)
- [Mostrar Evento](./EventShow.md)

## Changelog

- 2025-10-16: Documentación inicial
