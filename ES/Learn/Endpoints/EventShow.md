# Learn – Mostrar Evento

## Endpoint

```
GET /api/v1/learn/events/{event}
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

| Parámetro   | Tipo   | Requerido | Descripción | Predeterminado/Valores |
| ----------- | ------ | --------- | ----------- | ---------------------- |
| platform_id | string | No        | UUID de plataforma para validación de propiedad | Derivado de X-PUBLIC-KEY |

> El parámetro `event` usa vinculación de modelo de ruta con el campo `resource`. El endpoint acepta variaciones de nombres de parámetros (`camelCase`, `kebab-case`, `CapitalCase`).

## Ejemplos

### Ejemplo de solicitud (curl)

```bash
curl -X GET \
  -H "X-PUBLIC-KEY: <clave>" \
  -H "Accept-Language: es" \
  "https://sandbox.su-dominio.com/api/v1/learn/events/nombre-recurso-evento"
```

### Ejemplo de respuesta

```json
{
  "data": {
    "uuid": "880e8400-e29b-41d4-a716-446655440003",
    "resource": "nombre-recurso-evento",
    "platform_uuid": "aa0e8400-e29b-41d4-a716-446655440005",
    "contents_count": 12,
    "team_coordinators": [
      {
        "user_basic_data": {
          "uuid": "990e8400-e29b-41d4-a716-446655440004",
          "name": "Juan Pérez",
          "email": "juan@example.com",
          "images": [
            {
              "usage": "profile",
              "url": "https://example.com/profile.jpg"
            }
          ],
          "biography": {
            "job_title": {
              "title": "Instructor Senior"
            },
            "user": {
              "social_medias": [
                {
                  "platform": "linkedin",
                  "url": "https://linkedin.com/in/juanperez"
                }
              ]
            }
          }
        }
      }
    ],
    "regional_information": {
      "country": "ES",
      "state": "MD",
      "city": "Madrid"
    },
    "card": {
      "title": "Título de la Tarjeta del Evento",
      "description": "Descripción de la tarjeta del evento",
      "image_url": "https://example.com/card.jpg"
    },
    "titles": [
      {
        "language": "es",
        "value": "Curso Completo de Desarrollo Web",
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
      "valid_until": "2025-12-31T23:59:59Z",
      "titles": [
        {
          "language": "es",
          "value": "Oferta por Tiempo Limitado"
        }
      ]
    }
  }
}
```

## Estructura JSON Explicada

| Campo                               | Tipo    | Descripción |
| ----------------------------------- | ------- | ----------- |
| data                                | object  | Detalles del evento |
| data.uuid                           | string  | Identificador único del evento |
| data.resource                       | string  | Nombre del recurso del evento |
| data.platform_uuid                  | string  | UUID de la plataforma asociada |
| data.contents_count                 | integer | Número de contenidos en el evento |
| data.team_coordinators[]            | array   | Lista de coordinadores del equipo con detalles completos |
| data.team_coordinators[].user_basic_data | object | Información del usuario coordinador |
| data.regional_information           | object  | Información regional |
| data.card                           | object  | Información de visualización de tarjeta |
| data.titles[]                       | array   | Títulos localizados |
| data.prices[]                       | array   | Precios en diferentes monedas |
| data.offer                          | object  | Información de oferta activa (cuando disponible) |

## Estados HTTP

- 200: OK
- 400: Solicitud inválida
- 401: No autorizado
- 403: Prohibido - La plataforma no posee este evento
- 404: No encontrado - Evento no encontrado
- 429: Demasiadas solicitudes
- 500: Error interno del servidor

## Errores

```json
{
  "message": "Evento no encontrado"
}
```

```json
{
  "message": "Este evento no pertenece a la plataforma especificada"
}
```

## Notas

- Retorna información detallada para un evento específico
- El evento debe pertenecer a la plataforma que realiza la solicitud
- La propiedad de la plataforma se valida usando el parámetro `platform_id` o derivado de `X-PUBLIC-KEY`
- Incluye información completa de coordinadores del equipo con imágenes y biografías
- El contenido localizado se retorna según el encabezado `Accept-Language`
- La respuesta incluye ofertas activas cuando están disponibles

## Relacionados

- [Listar Eventos](./EventIndex.md)
- [Obtener Contador de Eventos](./EventCounter.md)
- [Obtener Texto del Evento](./EventText.md)
- [Listar Contenidos del Evento](./EventContentIndex.md)

## Changelog

- 2025-10-16: Documentación inicial
