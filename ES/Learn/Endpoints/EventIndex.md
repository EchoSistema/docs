# Learn – Listar Eventos

## Endpoint

```
GET /api/v1/learn/events
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

| Parámetro    | Tipo    | Requerido | Descripción | Predeterminado/Valores |
| ------------ | ------- | --------- | ----------- | ---------------------- |
| language     | string  | No        | Código de idioma para títulos localizados | Predeterminado de la plataforma |
| currency     | string  | No        | Código de moneda para precios (ej.: USD, BRL, EUR) | Predeterminado de la plataforma |
| per_page     | integer | No        | Ítems por página (paginación) | 10 |
| page         | integer | No        | Número de página | 1 |
| no_paginate  | boolean | No        | Retornar todos los resultados sin paginación | false |
| limit        | integer | No        | Máximo de ítems a retornar (cuando no_paginate=true) | Sin límite |

> Parámetros de filtro adicionales están disponibles a través de EventFilter. El endpoint acepta variaciones de nombres de parámetros (`camelCase`, `kebab-case`, `CapitalCase`).

## Ejemplos

### Ejemplo de solicitud (curl)

```bash
curl -X GET \
  -H "X-PUBLIC-KEY: <clave>" \
  -H "Accept-Language: es" \
  "https://sandbox.su-dominio.com/api/v1/learn/events?language=es&currency=USD&per_page=10&page=1"
```

### Ejemplo de respuesta

```json
{
  "data": [
    {
      "uuid": "880e8400-e29b-41d4-a716-446655440003",
      "resource": "nombre-recurso-evento",
      "contents_count": 12,
      "team_coordinators": [
        {
          "user_basic_data": {
            "uuid": "990e8400-e29b-41d4-a716-446655440004",
            "name": "Juan Pérez",
            "images": [
              {
                "usage": "profile",
                "url": "https://example.com/profile.jpg"
              }
            ]
          }
        }
      ],
      "regional_information": {
        "country": "ES",
        "state": "MD"
      },
      "card": {
        "title": "Título de la Tarjeta del Evento",
        "description": "Descripción de la tarjeta del evento"
      },
      "titles": [
        {
          "language": "es",
          "value": "Título del Evento",
          "is_default": false
        }
      ],
      "prices": [
        {
          "currency_id": "EUR",
          "amount": 89.99,
          "is_default": false
        }
      ],
      "offer": {
        "is_active": true,
        "discount_percent": 20,
        "titles": [
          {
            "language": "es",
            "value": "Oferta Especial"
          }
        ]
      }
    }
  ],
  "links": {
    "first": "https://sandbox.su-dominio.com/api/v1/learn/events?page=1",
    "last": "https://sandbox.su-dominio.com/api/v1/learn/events?page=5",
    "prev": null,
    "next": "https://sandbox.su-dominio.com/api/v1/learn/events?page=2"
  },
  "meta": {
    "current_page": 1,
    "from": 1,
    "last_page": 5,
    "path": "https://sandbox.su-dominio.com/api/v1/learn/events",
    "per_page": 10,
    "to": 10,
    "total": 47
  }
}
```

## Estructura JSON Explicada

| Campo                          | Tipo    | Descripción |
| ------------------------------ | ------- | ----------- |
| data[]                         | array   | Lista de eventos |
| data[].uuid                    | string  | Identificador único del evento |
| data[].resource                | string  | Nombre del recurso del evento |
| data[].contents_count          | integer | Número de contenidos en el evento |
| data[].team_coordinators[]     | array   | Lista de coordinadores del equipo |
| data[].regional_information    | object  | Información regional |
| data[].card                    | object  | Información de visualización de tarjeta |
| data[].titles[]                | array   | Títulos localizados |
| data[].prices[]                | array   | Precios en diferentes monedas |
| data[].offer                   | object  | Información de oferta activa |
| links                          | object  | Enlaces de paginación |
| meta                           | object  | Metadatos de paginación |

## Estados HTTP

- 200: OK
- 400: Solicitud inválida
- 401: No autorizado
- 403: Prohibido
- 404: No encontrado
- 422: Entidad no procesable
- 429: Demasiadas solicitudes
- 500: Error interno del servidor

## Errores

```json
{
  "message": "Clave de plataforma inválida",
  "errors": {
    "per_page": ["El per page debe ser un número entero."]
  }
}
```

## Notas

- Retorna todos los eventos para la plataforma autenticada
- Los resultados están paginados por defecto con 10 ítems por página
- Use `no_paginate=true` para recuperar todos los resultados de una vez (usar con precaución)
- Los títulos y precios están localizados según los parámetros `language` y `currency`
- Si el contenido localizado no está disponible, se retornan valores predeterminados
- La respuesta incluye coordinadores del equipo con sus imágenes de perfil y biografías
- Las ofertas activas se incluyen en la respuesta cuando están disponibles

## Relacionados

- [Mostrar Evento](./EventShow.md)
- [Obtener Contador de Eventos](./EventCounter.md)
- [Listar Contenidos del Evento](./EventContentIndex.md)

## Changelog

- 2025-10-16: Documentación inicial
