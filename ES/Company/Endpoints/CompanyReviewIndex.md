# Company – Listar Reseñas de Empresas

## Endpoint

```
GET /api/v1/company/reviews
GET /api/v1/company/reviews/my
```

## Autenticación

- `/api/v1/company/reviews` - Ninguna
- `/api/v1/company/reviews/my` - Requerida - Bearer {token} con habilidad `backoffice`

## Encabezados

| Encabezado       | Tipo   | Requerido | Descripción |
| ---------------- | ------ | --------- | ----------- |
| Authorization    | string | Para ruta `/my` | Credencial `Bearer {token}`. |
| X-PUBLIC-KEY     | string | Sí        | Clave pública de la plataforma. |
| Accept-Language  | string | No        | Locale IETF (ej.: `pt-BR`, `en`, `es`). |

## Parámetros

### Parámetros de consulta

| Parámetro | Tipo    | Requerido | Descripción | Predeterminado/Valores |
| --------- | ------- | --------- | ----------- | ---------------------- |
| is_public | boolean | No | Filtrar solo reseñas públicas. Se establece automáticamente en true si no está autenticado. | - |
| company | string | No | Filtrar por UUID o slug de empresa. | - |
| user | string | No | Filtrar por UUID de usuario. | - |
| keywords | string | No | Buscar en el texto de la reseña. | - |
| rating | integer | No | Filtrar por valor de calificación. | 1-5 |
| created_at | date | No | Filtrar por fecha de creación. | fecha ISO 8601 |
| interval | string | No | Filtro de intervalo de tiempo. | hour, day, week, month, year |
| period | object | No | Filtro de rango de fechas. | - |
| period.from | date | No | Fecha de inicio del período. | fecha ISO 8601 |
| period.to | date | No | Fecha de fin del período. | fecha ISO 8601 |
| with_images | boolean | No | Filtrar solo reseñas con imágenes. | - |
| sort_by | string | No | Campo para ordenar. | created_at |
| order_by | string | No | Dirección de ordenamiento. | desc (ASC, DESC) |
| page | integer | No | Número de página para paginación. | 1 |
| per_page | integer | No | Ítems por página. | 25 |
| no_paginate | boolean | No | Deshabilitar paginación. | false |
| limit | integer | No | Limitar resultados cuando `no_paginate` es true. | - |

> Los nombres de parámetros están documentados en `snake_case`. El endpoint acepta variantes (`camelCase`, `kebab-case`, `CapitalCase`).

## Ejemplos

### Ejemplo de solicitud (curl)

```bash
curl -X GET \
  -H "X-PUBLIC-KEY: <clave>" \
  -H "Accept-Language: es" \
  "https://sandbox.su-dominio.com/api/v1/company/reviews?rating=5&per_page=10"
```

### Ejemplo de respuesta

```json
{
  "data": [
    {
      "id": 1,
      "uuid": "123e4567-e89b-12d3-a456-426614174000",
      "review": "Excelente servicio y productos de calidad.",
      "rating": 5,
      "is_public": true,
      "created_at": "2025-01-15T10:30:00.000000Z",
      "company": {
        "id": 1,
        "uuid": "223e4567-e89b-12d3-a456-426614174000",
        "name": "Empresa Ejemplo",
        "slug": "empresa-ejemplo",
        "rating": {
          "average": 4.5,
          "total": 120
        },
        "logos": [
          {
            "url": "https://example.com/logo.png",
            "usage": "logo"
          }
        ]
      }
    }
  ],
  "links": {
    "first": "https://sandbox.su-dominio.com/api/v1/company/reviews?page=1",
    "last": "https://sandbox.su-dominio.com/api/v1/company/reviews?page=10",
    "prev": null,
    "next": "https://sandbox.su-dominio.com/api/v1/company/reviews?page=2"
  },
  "meta": {
    "current_page": 1,
    "from": 1,
    "last_page": 10,
    "per_page": 10,
    "to": 10,
    "total": 100
  }
}
```

## Estructura JSON Explicada

| Campo       | Tipo    | Descripción |
| ----------- | ------- | ----------- |
| data[] | array | Lista de reseñas. |
| data[].id | integer | ID de reseña. |
| data[].uuid | string | UUID de reseña. |
| data[].review | string | Contenido de texto de la reseña. |
| data[].rating | integer | Valor de calificación (1-5). |
| data[].is_public | boolean | Si la reseña es pública. |
| data[].created_at | string | Fecha de creación de reseña (ISO 8601). |
| data[].company | object | Información de la empresa. |
| data[].company.id | integer | ID de empresa. |
| data[].company.uuid | string | UUID de empresa. |
| data[].company.name | string | Nombre de empresa. |
| data[].company.slug | string | Slug de empresa. |
| data[].company.rating | object | Información de calificación de empresa. |
| data[].company.rating.average | float | Calificación promedio. |
| data[].company.rating.total | integer | Número total de calificaciones. |
| data[].company.logos[] | array | Logos de empresa. |
| links | object | Enlaces de paginación. |
| meta | object | Metadatos de paginación. |

## Estados HTTP

- 200: OK
- 400: Solicitud inválida
- 401: No autorizado (para ruta `/my` sin autenticación)
- 422: Entidad no procesable
- 429: Demasiadas solicitudes
- 500: Error interno del servidor

## Errores

Errores de validación estándar:

```json
{
  "message": "Los datos proporcionados no son válidos.",
  "errors": {
    "rating": ["La calificación debe estar entre 1 y 5."]
  }
}
```

## Notas

- Las solicitudes no autenticadas filtran automáticamente solo reseñas públicas.
- La ruta `/my` devuelve solo reseñas del usuario autenticado.
- Los usuarios con rol de invitado solo pueden ver sus propias reseñas.
- La paginación predeterminada es de 25 ítems por página.
- Las reseñas incluyen datos de empresa relacionados con logos e información de calificación.
- Use el parámetro `keywords` para búsqueda de texto completo en el contenido de reseñas.

## Relacionados

- [Crear Reseña de Empresa](CompanyReviewStore.md)
- [Mostrar Empresa](CompanyShow.md)
- [Listar Empresas por Cobertura](CompanyThroughCoverageIndex.md)
