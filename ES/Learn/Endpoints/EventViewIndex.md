# Learn – Listar Vistas de Eventos

## Endpoint

```
GET /api/v1/learn/v/events
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

### Parámetros de consulta

| Parámetro      | Tipo   | Requerido | Descripción | Predeterminado/Valores |
| -------------- | ------ | --------- | ----------- | ---------------------- |
| platform_uuid  | string | No        | UUID de plataforma para filtrado | Derivado de X-PUBLIC-KEY |

> El endpoint acepta variaciones de nombres de parámetros (`camelCase`, `kebab-case`, `CapitalCase`).

## Ejemplos

### Ejemplo de solicitud (curl)

```bash
curl -X GET \
  -H "X-PUBLIC-KEY: <clave>" \
  -H "Accept-Language: es" \
  "https://sandbox.su-dominio.com/api/v1/learn/v/events"
```

### Ejemplo de respuesta

```json
{
  "data": [
    {
      "_id": "507f1f77bcf86cd799439011",
      "event_uuid": "880e8400-e29b-41d4-a716-446655440003",
      "platform_uuid": "aa0e8400-e29b-41d4-a716-446655440005",
      "resource": "nombre-recurso-evento",
      "title": "Curso Completo de Desarrollo Web",
      "description": "Aprende desarrollo web desde cero",
      "contents_count": 12,
      "total_duration_minutes": 540,
      "category": "Programación",
      "is_active": true,
      "created_at": "2025-01-15T10:30:00Z",
      "updated_at": "2025-10-10T14:20:00Z"
    },
    {
      "_id": "507f1f77bcf86cd799439012",
      "event_uuid": "990e8400-e29b-41d4-a716-446655440011",
      "platform_uuid": "aa0e8400-e29b-41d4-a716-446655440005",
      "resource": "otro-evento",
      "title": "JavaScript Avanzado",
      "description": "Domina JavaScript moderno",
      "contents_count": 8,
      "total_duration_minutes": 360,
      "category": "Programación",
      "is_active": true,
      "created_at": "2025-02-20T08:00:00Z",
      "updated_at": "2025-09-15T16:45:00Z"
    }
  ]
}
```

## Estructura JSON Explicada

| Campo                       | Tipo    | Descripción |
| --------------------------- | ------- | ----------- |
| data[]                      | array   | Lista de vistas materializadas de eventos |
| data[]._id                  | string  | Identificador de documento MongoDB |
| data[].event_uuid           | string  | Identificador único del evento |
| data[].platform_uuid        | string  | UUID de la plataforma asociada |
| data[].resource             | string  | Nombre del recurso del evento |
| data[].title                | string  | Título del evento |
| data[].description          | string  | Descripción del evento |
| data[].contents_count       | integer | Número de contenidos |
| data[].total_duration_minutes | integer | Duración total en minutos |
| data[].category             | string  | Categoría del evento |
| data[].is_active            | boolean | Si el evento está activo |
| data[].created_at           | string  | Marca de tiempo de creación (ISO 8601) |
| data[].updated_at           | string  | Marca de tiempo de última actualización (ISO 8601) |

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

- Retorna vistas materializadas de eventos desde MongoDB para rendimiento de lectura optimizado
- Las vistas materializadas proporcionan datos desnormalizados y pre-computados para consultas más rápidas
- Los datos se leen de una base de datos NoSQL (MongoDB) separada de la base de datos relacional principal
- Los eventos se filtran por `platform_uuid` que se deriva del encabezado `X-PUBLIC-KEY`
- Este endpoint está optimizado para operaciones de listado y búsqueda
- La vista puede no reflejar cambios en tiempo real; las actualizaciones se sincronizan de forma asíncrona
- Útil para paneles y reportes donde el rendimiento de lectura es crítico

## Relacionados

- [Listar Eventos](./EventIndex.md)
- [Mostrar Evento](./EventShow.md)

## Changelog

- 2025-10-16: Documentación inicial
