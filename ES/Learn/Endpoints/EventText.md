# Learn – Obtener Texto del Evento

## Endpoint

```
GET /api/v1/learn/events/{event}/text/{usage}
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
| event     | string | Sí        | Identificador de texto del evento (no el nombre del recurso) |
| usage     | string | Sí        | Tipo de uso del texto (ej.: description, about, terms) |

### Parámetros de consulta

| Parámetro   | Tipo   | Requerido | Descripción | Predeterminado/Valores |
| ----------- | ------ | --------- | ----------- | ---------------------- |
| platform_id | string | No        | UUID de plataforma para validación de propiedad | Derivado de X-PUBLIC-KEY |

> El parámetro `event` usa vinculación de campo `text`. El endpoint acepta variaciones de nombres de parámetros (`camelCase`, `kebab-case`, `CapitalCase`).

## Ejemplos

### Ejemplo de solicitud (curl)

```bash
curl -X GET \
  -H "X-PUBLIC-KEY: <clave>" \
  -H "Accept-Language: es" \
  "https://sandbox.su-dominio.com/api/v1/learn/events/evento-texto-123/text/description"
```

### Ejemplo de respuesta

```json
{
  "data": {
    "uuid": "bb0e8400-e29b-41d4-a716-446655440006",
    "usage": "description",
    "content": "Este es un curso completo que cubre todos los aspectos del desarrollo web...",
    "language": "es",
    "is_default": false,
    "created_at": "2025-01-15T10:30:00Z",
    "updated_at": "2025-10-10T14:20:00Z"
  }
}
```

## Estructura JSON Explicada

| Campo              | Tipo    | Descripción |
| ------------------ | ------- | ----------- |
| data               | object  | Datos del contenido de texto |
| data.uuid          | string  | Identificador único del texto |
| data.usage         | string  | Tipo de uso del texto |
| data.content       | string  | Contenido del texto |
| data.language      | string  | Código de idioma del texto |
| data.is_default    | boolean | Si este es el texto predeterminado |
| data.created_at    | string  | Marca de tiempo de creación (ISO 8601) |
| data.updated_at    | string  | Marca de tiempo de última actualización (ISO 8601) |

## Estados HTTP

- 200: OK
- 400: Solicitud inválida
- 401: No autorizado
- 403: Prohibido - La plataforma no posee este evento
- 404: No encontrado - Evento o texto no encontrado
- 429: Demasiadas solicitudes
- 500: Error interno del servidor

## Errores

```json
{
  "message": "Texto no encontrado"
}
```

```json
{
  "message": "Este evento no pertenece a la plataforma especificada"
}
```

## Notas

- Retorna un contenido de texto específico para un evento según el tipo de uso
- El evento debe pertenecer a la plataforma que realiza la solicitud
- La propiedad de la plataforma se valida usando el parámetro `platform_id` o derivado de `X-PUBLIC-KEY`
- Los tipos de uso comunes incluyen: `description`, `about`, `terms`, `prerequisites`, `objectives`
- El contenido del texto se retorna según el encabezado `Accept-Language` cuando está disponible

## Relacionados

- [Mostrar Evento](./EventShow.md)
- [Listar Eventos](./EventIndex.md)

## Changelog

- 2025-10-16: Documentación inicial
