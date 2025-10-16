# Learn – Listar Sub-Contenidos del Evento

## Endpoint

```
GET /api/v1/learn/events/{event}/contents/{index}/sub-contents
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
  "https://sandbox.su-dominio.com/api/v1/learn/events/nombre-recurso-evento/contents/1/sub-contents?language=es"
```

### Ejemplo de respuesta

```json
{
  "data": [
    {
      "uuid": "ee0e8400-e29b-41d4-a716-446655440009",
      "index_number": 1,
      "parent_content_index": 1,
      "content_type": "video",
      "duration_minutes": 15,
      "title": {
        "language": "es",
        "value": "Fundamentos de HTML - Parte 1",
        "is_default": false
      },
      "description": {
        "language": "es",
        "value": "Introducción a las etiquetas y estructura HTML"
      },
      "media": {
        "url": "https://example.com/sub-video-1.mp4",
        "thumbnail": "https://example.com/sub-thumb-1.jpg"
      }
    },
    {
      "uuid": "ff0e8400-e29b-41d4-a716-446655440010",
      "index_number": 2,
      "parent_content_index": 1,
      "content_type": "quiz",
      "duration_minutes": 5,
      "title": {
        "language": "es",
        "value": "Verificación de Conocimiento HTML"
      }
    }
  ]
}
```

## Estructura JSON Explicada

| Campo                        | Tipo    | Descripción |
| ---------------------------- | ------- | ----------- |
| data[]                       | array   | Lista de sub-contenidos |
| data[].uuid                  | string  | Identificador único del sub-contenido |
| data[].index_number          | integer | Índice secuencial del sub-contenido |
| data[].parent_content_index  | integer | Número de índice del contenido padre |
| data[].content_type          | string  | Tipo de sub-contenido |
| data[].duration_minutes      | integer | Duración estimada en minutos |
| data[].title                 | object  | Título localizado del sub-contenido |
| data[].description           | object  | Descripción localizada del sub-contenido |
| data[].media                 | object  | URLs de medios (cuando aplica) |

## Estados HTTP

- 200: OK
- 400: Solicitud inválida
- 401: No autorizado - Autenticación requerida
- 403: Prohibido - La plataforma no posee este evento
- 404: No encontrado - Evento o contenido no encontrado
- 429: Demasiadas solicitudes
- 500: Error interno del servidor

## Errores

```json
{
  "message": "Contenido no encontrado"
}
```

```json
{
  "message": "No autenticado"
}
```

## Notas

- Retorna todos los sub-contenidos para un contenido padre específico
- Requiere autenticación ya que los sub-contenidos generalmente no son contenido de vista previa
- El evento debe pertenecer a la plataforma que realiza la solicitud
- La propiedad de la plataforma se valida usando el parámetro `platform_id` o derivado de `X-PUBLIC-KEY`
- Los sub-contenidos están ordenados por `index_number`
- Los títulos y descripciones están localizados según el parámetro `language` o encabezado `Accept-Language`
- Si el contenido localizado no está disponible, se retornan valores predeterminados

## Relacionados

- [Mostrar Sub-Contenido del Evento](./EventContentSubContentShow.md)
- [Mostrar Contenido del Evento](./EventContentShow.md)
- [Listar Contenidos del Evento](./EventContentIndex.md)

## Changelog

- 2025-10-16: Documentación inicial
