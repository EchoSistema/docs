# Learn – Obtener Contador de Eventos

## Endpoint

```
GET /api/v1/learn/events/counter
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

Parámetros de filtro adicionales están disponibles a través de EventCounterFilter.

> El endpoint acepta variaciones de nombres de parámetros (`camelCase`, `kebab-case`, `CapitalCase`).

## Ejemplos

### Ejemplo de solicitud (curl)

```bash
curl -X GET \
  -H "X-PUBLIC-KEY: <clave>" \
  -H "Accept-Language: es" \
  "https://sandbox.su-dominio.com/api/v1/learn/events/counter"
```

### Ejemplo de respuesta

```json
{
  "data": {
    "total": 47,
    "active": 42,
    "inactive": 5,
    "by_category": {
      "programacion": 25,
      "diseno": 12,
      "negocios": 10
    }
  }
}
```

## Estructura JSON Explicada

| Campo                  | Tipo    | Descripción |
| ---------------------- | ------- | ----------- |
| data                   | object  | Datos del contador |
| data.total             | integer | Número total de eventos |
| data.active            | integer | Número de eventos activos |
| data.inactive          | integer | Número de eventos inactivos |
| data.by_category       | object  | Conteo de eventos agrupados por categoría |

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

- Retorna conteos agregados de eventos para la plataforma
- Soporta filtrado a través de EventCounterFilter
- Útil para estadísticas de panel y análisis
- El contador respeta la propiedad de la plataforma y solo cuenta eventos pertenecientes a la plataforma autenticada

## Relacionados

- [Listar Eventos](./EventIndex.md)
- [Mostrar Evento](./EventShow.md)

## Changelog

- 2025-10-16: Documentación inicial
