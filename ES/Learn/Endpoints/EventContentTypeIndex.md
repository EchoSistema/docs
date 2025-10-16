# Learn – Listar Tipos de Contenido de Evento

## Endpoint

```
GET /api/v1/learn/events/content-types
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

No se requieren parámetros adicionales.

## Ejemplos

### Ejemplo de solicitud (curl)

```bash
curl -X GET \
  -H "X-PUBLIC-KEY: <clave>" \
  -H "Accept-Language: es" \
  "https://sandbox.su-dominio.com/api/v1/learn/events/content-types"
```

### Ejemplo de respuesta

```json
{
  "data": [
    {
      "id": "video",
      "name": "Video"
    },
    {
      "id": "audio",
      "name": "Audio"
    },
    {
      "id": "text",
      "name": "Texto"
    },
    {
      "id": "pdf",
      "name": "Documento PDF"
    },
    {
      "id": "quiz",
      "name": "Cuestionario"
    },
    {
      "id": "assignment",
      "name": "Tarea"
    }
  ]
}
```

## Estructura JSON Explicada

| Campo       | Tipo   | Descripción |
| ----------- | ------ | ----------- |
| data[]      | array  | Lista de tipos de contenido |
| data[].id   | string | Identificador del tipo de contenido |
| data[].name | string | Nombre localizado del tipo de contenido |

## Estados HTTP

- 200: OK
- 400: Solicitud inválida
- 401: No autorizado
- 403: Prohibido
- 404: No encontrado
- 429: Demasiadas solicitudes
- 500: Error interno del servidor

## Errores

```json
{
  "message": "Clave de plataforma inválida"
}
```

## Notas

- Retorna todos los tipos de contenido de evento disponibles
- Los nombres de tipos de contenido están localizados según el encabezado `Accept-Language`
- La lista está ordenada alfabéticamente por nombre
- Estos tipos se utilizan al crear o filtrar contenidos de eventos
- Los tipos de contenido comunes incluyen: video, audio, texto, PDF, cuestionario, tarea, interactivo, sesión en vivo

## Relacionados

- [Listar Contenidos del Evento](./EventContentIndex.md)
- [Mostrar Contenido del Evento](./EventContentShow.md)

## Changelog

- 2025-10-16: Documentación inicial
