# Cms – Crear Título

## Endpoint

```
POST /api/v1/cms/titles
```

## Autenticación

Requerida – Bearer {token} con habilidad `store.platform.title`

## Encabezados

| Header           | Type   | Required | Description |
| ---------------- | ------ | -------- | ----------- |
| Authorization    | string | Sí       | `Bearer {token}`. |
| X-PUBLIC-KEY     | string | Sí       | Clave pública de la plataforma. |
| Accept-Language  | string | No       | Locale IETF (ej., `pt-BR`, `en`, `es`). |

## Parámetros

### Parámetros de ruta

Ninguno.

### Cuerpo de la solicitud

| Parámetro       | Tipo    | Requerido | Descripción | Valores |
| --------------- | ------- | --------- | ----------- | ------- |
| language        | string  | Sí        | Código de idioma (ISO 639-1). | `en`, `es`, `pt`, `gn` |
| is_default      | boolean | Sí        | Si es la traducción predeterminada. | |
| content         | string  | Sí        | Contenido del título (máx. 255 caracteres). | |
| title           | string  | No        | Alias para el campo `content`. | |
| usage           | string  | No        | Identificador de contexto de uso (máx. 255 caracteres). | |
| additional_data | object  | No        | Objeto de datos adicionales. | |
| additional_data.subtitle | string | No | Texto del subtítulo (máx. 255 caracteres). | |
| additional_data.text     | string | No | Contenido de texto extendido (máx. 5000 caracteres). | |

> Los nombres de parámetros aceptan variantes `snake_case`, `camelCase`, `kebab-case` o `CapitalCase`.
>
> **Nota:** El campo `additional_data` se convertirá a `raw` internamente por el manejador de solicitud.

## Ejemplos

### Ejemplo de solicitud (curl)

```bash
curl -X POST \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: es" \
  -H "Content-Type: application/json" \
  -d '{
    "language": "es",
    "is_default": true,
    "content": "Bienvenido a Nuestra Plataforma",
    "usage": "homepage.hero",
    "additional_data": {
      "subtitle": "Tu viaje comienza aquí",
      "text": "Descubre increíbles características y servicios diseñados para ayudarte a tener éxito."
    }
  }' \
  "https://sandbox.your-domain.com/api/v1/cms/titles"
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
    }
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

## Estado HTTP

- 201: Creado
- 400: Solicitud Incorrecta
- 401: No Autorizado
- 403: Prohibido
- 404: No Encontrado
- 422: Entidad No Procesable
- 429: Demasiadas Solicitudes
- 500: Error Interno del Servidor

## Errores

```json
{
  "message": "Los datos proporcionados no son válidos.",
  "errors": {
    "language": ["El campo language es obligatorio."],
    "is_default": ["El campo is default es obligatorio."],
    "content": ["El campo content es obligatorio."]
  }
}
```

## Notas

- El usuario autenticado debe tener la habilidad `store.platform.title`.
- Se puede usar el campo `content` o `title`; son alias.
- El objeto `additional_data` se convierte al campo `raw` internamente.
- Longitud máxima para `content` es 255 caracteres.
- Longitud máxima para `usage` es 255 caracteres.
- Longitud máxima para `additional_data.subtitle` es 255 caracteres.
- Longitud máxima para `additional_data.text` es 5000 caracteres.
- Todas las marcas de tiempo están en formato ISO 8601.

## Relacionados

- [Índice de Títulos Cms](./CmsTitleIndex.md)
- [Mostrar Título Cms](./CmsTitleShow.md)
- [Dominio Cms](../README.md)

## Changelog

- 2025-10-23: Actualización con documentación completa de solicitud/respuesta.
- 2025-10-16: Documentación inicial.
