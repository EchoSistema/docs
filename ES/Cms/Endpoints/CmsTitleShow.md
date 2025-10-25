# Cms – Mostrar Título

## Endpoint

```
GET /api/v1/cms/titles/{title}
```

## Autenticación

Ninguna

## Encabezados

| Header           | Type   | Required | Description |
| ---------------- | ------ | -------- | ----------- |
| X-PUBLIC-KEY     | string | Sí       | Clave pública de la plataforma. |
| Accept-Language  | string | No       | Locale IETF (ej., `pt-BR`, `en`, `es`). |

## Parámetros

### Parámetros de ruta

| Parámetro | Tipo   | Requerido | Descripción |
| --------- | ------ | --------- | ----------- |
| title     | string | Sí        | Identificador UUID del título. |

### Parámetros de consulta

| Parámetro | Tipo   | Requerido | Descripción | Valores |
| --------- | ------ | --------- | ----------- | ------- |
| language  | string | No        | Filtrar relaciones por código de idioma. | |

> Los nombres de parámetros aceptan variantes `snake_case`, `camelCase`, `kebab-case` o `CapitalCase`.

## Ejemplos

### Ejemplo de solicitud (curl)

```bash
curl -X GET \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: es" \
  "https://sandbox.your-domain.com/api/v1/cms/titles/9d4e8f2a-1234-5678-90ab-cdef12345678?language=es"
```

### Ejemplo de respuesta

```json
{
  "data": {
    "uuid": "9d4e8f2a-1234-5678-90ab-cdef12345678",
    "language": "es",
    "is_default": true,
    "content": "Bienvenido a Nuestra Plataforma",
    "usage": "homepage.hero",
    "raw": {
      "subtitle": "Tu viaje comienza aquí",
      "text": "Descubre increíbles características y servicios diseñados para ayudarte a tener éxito."
    },
    "created_at": "2025-10-23T12:00:00Z",
    "updated_at": "2025-10-23T12:00:00Z",
    "text": {
      "uuid": "8c3d7e1b-5678-90ab-cdef-123456789012",
      "language": "es",
      "is_default": true,
      "content": "Bienvenido a Nuestra Plataforma"
    },
    "titles": [
      {
        "uuid": "7b2c6d0a-4567-89ab-cdef-0123456789ab",
        "language": "es",
        "is_default": true,
        "content": "Descripción de la Plataforma"
      }
    ]
  }
}
```

## Estructura JSON Explicada

| Campo              | Tipo     | Descripción |
| ------------------ | -------- | ----------- |
| data               | object   | Recurso de título. |
| data.uuid          | string   | Identificador UUID del título. |
| data.language      | string   | Código de idioma (ISO 639-1). |
| data.is_default    | boolean  | Si es la traducción predeterminada. |
| data.content       | string   | Contenido del título. |
| data.usage         | string   | Identificador de contexto de uso. |
| data.raw           | object   | Datos adicionales almacenados como JSON. |
| data.raw.subtitle  | string   | Texto del subtítulo. |
| data.raw.text      | string   | Contenido de texto extendido. |
| data.created_at    | datetime | Marca de tiempo de creación en formato ISO 8601. |
| data.updated_at    | datetime | Marca de tiempo de última actualización en formato ISO 8601. |
| data.text          | object   | Recurso de texto relacionado. |
| data.text.uuid     | string   | Identificador UUID del texto. |
| data.text.language | string   | Código de idioma del texto. |
| data.text.is_default | boolean | Si el texto es traducción predeterminada. |
| data.text.content  | string   | Contenido del texto. |
| data.titles        | array    | Colección de títulos relacionados. |
| data.titles[].uuid | string   | Identificador UUID del título relacionado. |
| data.titles[].language | string | Código de idioma del título relacionado. |
| data.titles[].is_default | boolean | Si el título relacionado es traducción predeterminada. |
| data.titles[].content | string | Contenido del título relacionado. |

## Estado HTTP

- 200: OK
- 400: Solicitud Incorrecta
- 401: No Autorizado
- 403: Prohibido
- 404: No Encontrado
- 422: Entidad No Procesable
- 429: Demasiadas Solicitudes
- 500: Error Interno del Servidor

## Notas

- El parámetro de ruta `title` debe ser un UUID válido.
- Cuando se proporciona `language`, se cargarán las relaciones `text` y `titles` para ese idioma o traducciones predeterminadas.
- Todas las marcas de tiempo están en formato ISO 8601.

## Relacionados

- [Listar Títulos Cms](./CmsTitleIndex.md)
- [Crear Título Cms](./CmsTitleStore.md)
- [Dominio Cms](../README.md)

## Changelog

- 2025-10-23: Actualización con documentación completa de respuesta y parámetros de consulta.
- 2025-10-16: Documentación inicial.
