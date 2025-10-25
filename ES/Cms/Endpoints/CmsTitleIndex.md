# Cms – Listar Títulos

## Endpoint

```
GET /api/v1/cms/titles
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

Ninguno.

### Parámetros de consulta

| Parámetro      | Tipo    | Requerido | Descripción | Valores |
| -------------- | ------- | --------- | ----------- | ------- |
| language       | string  | No        | Filtrar por código de idioma. | |
| uuid           | string  | No        | Filtrar por UUID. | |
| all_language   | boolean | No        | Incluir todos los idiomas. | `false` |
| is_default     | boolean | No        | Filtrar por traducción predeterminada. | |
| usage          | string  | No        | Filtrar por contexto de uso exacto. | |
| usage_like     | string  | No        | Filtrar por contexto de uso (coincidencia parcial). | |
| with_text      | boolean | No        | Incluir relación de texto. | `false` |
| with_texts     | boolean | No        | Incluir relación de textos. | `false` |
| per_page       | integer | No        | Resultados por página. | `25` (1-100) |
| page           | integer | No        | Número de página. | `1` |
| no_paginate    | boolean | No        | Deshabilitar paginación. | `false` |
| sort           | string  | No        | Campo de ordenamiento. | `content` |
| direction      | string  | No        | Dirección de ordenamiento. | `asc` (`asc`, `desc`) |

> Los nombres de parámetros aceptan variantes `snake_case`, `camelCase`, `kebab-case` o `CapitalCase`.

## Ejemplos

### Ejemplo de solicitud (curl)

```bash
curl -X GET \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: es" \
  "https://sandbox.your-domain.com/api/v1/cms/titles?language=es&usage=homepage.hero&per_page=10&page=1"
```

### Ejemplo de respuesta

```json
{
  "data": [
    {
      "uuid": "9d4e8f2a-1234-5678-90ab-cdef12345678",
      "language": "es",
      "is_default": true,
      "content": "Bienvenido a Nuestra Plataforma",
      "usage": "homepage.hero",
      "created_at": "2025-10-23T12:00:00Z",
      "updated_at": "2025-10-23T12:00:00Z",
      "text": {
        "uuid": "8c3d7e1b-5678-90ab-cdef-123456789012",
        "language": "es",
        "is_default": true,
        "content": "Bienvenido a Nuestra Plataforma"
      }
    }
  ],
  "links": {
    "first": "https://sandbox.your-domain.com/api/v1/cms/titles?page=1",
    "last": "https://sandbox.your-domain.com/api/v1/cms/titles?page=5",
    "prev": null,
    "next": "https://sandbox.your-domain.com/api/v1/cms/titles?page=2"
  },
  "meta": {
    "current_page": 1,
    "from": 1,
    "last_page": 5,
    "path": "https://sandbox.your-domain.com/api/v1/cms/titles",
    "per_page": 10,
    "to": 10,
    "total": 50
  }
}
```

## Estructura JSON Explicada

| Campo              | Tipo     | Descripción |
| ------------------ | -------- | ----------- |
| data               | array    | Colección de recursos de título. |
| data[].uuid        | string   | Identificador UUID del título. |
| data[].language    | string   | Código de idioma (ISO 639-1). |
| data[].is_default  | boolean  | Si es la traducción predeterminada. |
| data[].content     | string   | Contenido del título. |
| data[].usage       | string   | Identificador de contexto de uso. |
| data[].created_at  | datetime | Marca de tiempo de creación en formato ISO 8601. |
| data[].updated_at  | datetime | Marca de tiempo de última actualización en formato ISO 8601. |
| data[].text        | object   | Recurso de texto relacionado (cuando se solicita). |
| data[].text.uuid   | string   | Identificador UUID del texto. |
| data[].text.language | string | Código de idioma del texto. |
| data[].text.is_default | boolean | Si el texto es traducción predeterminada. |
| data[].text.content | string  | Contenido del texto. |
| data[].titles      | array    | Colección de títulos relacionados (cuando se solicita). |
| links              | object   | Enlaces de paginación. |
| meta               | object   | Metadatos de paginación. |

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

- El parámetro `language` filtra títulos por el código de idioma especificado.
- Cuando se proporciona `language`, se cargarán las relaciones `text` y `titles` para ese idioma o traducciones predeterminadas.
- El parámetro `usage_like` permite coincidencia parcial en el campo de uso.
- El parámetro `no_paginate` devuelve todos los resultados sin paginación.
- El orden predeterminado es por `content` en orden ascendente.
- Todas las marcas de tiempo están en formato ISO 8601.

## Relacionados

- [Crear Título Cms](./CmsTitleStore.md)
- [Mostrar Título Cms](./CmsTitleShow.md)
- [Dominio Cms](../README.md)

## Changelog

- 2025-10-23: Actualización con parámetros de consulta completos y documentación de respuesta.
- 2025-10-16: Documentación inicial.
