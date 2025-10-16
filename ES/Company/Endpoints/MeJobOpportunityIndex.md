# Company – Listar Oportunidades de Empleo

## Endpoint

```
GET /api/v1/company/opportunities
```

## Autenticación

Ninguna

## Encabezados

| Encabezado       | Tipo   | Requerido | Descripción |
| ---------------- | ------ | --------- | ----------- |
| X-PUBLIC-KEY     | string | Sí        | Clave pública de la plataforma. |
| Accept-Language  | string | No        | Locale IETF (ej.: `pt-BR`, `en`, `es`). |

## Parámetros

### Parámetros de consulta

| Parámetro   | Tipo    | Requerido | Descripción | Predeterminado/Valores |
| ----------- | ------- | --------- | ----------- | ---------------------- |
| language    | string  | No        | Código de idioma para campos traducibles (ej.: `en`, `pt-BR`, `es`). | Predeterminado de la plataforma |
| noPaginate  | boolean | No        | Deshabilitar paginación cuando es `true`. | `false` |
| perPage     | integer | No        | Número de ítems por página cuando está paginado. | 25 |
| page        | integer | No        | Número de página cuando está paginado. | 1 |
| withTrash   | boolean | No        | Incluir oportunidades eliminadas. | `false` |

> Los nombres de parámetros aceptan variantes `snake_case`, `camelCase`, `kebab-case`, o `CapitalCase`.

## Ejemplos

### Ejemplo de solicitud (curl)

```bash
curl -X GET \
  -H "X-PUBLIC-KEY: <clave>" \
  -H "Accept-Language: es" \
  "https://sandbox.su-dominio.com/api/v1/company/opportunities?language=es&perPage=10"
```

### Ejemplo de respuesta (paginada)

```json
{
  "data": [
    {
      "uuid": "00000000-0000-0000-0000-000000000001",
      "company": "Tech Solutions Inc.",
      "total_job_openings": 3,
      "title": "Desarrollador de Software Senior",
      "mode": "remote",
      "starts_at": "2025-10-20T00:00:00+00:00",
      "finishes_at": "2025-11-30T23:59:59+00:00"
    },
    {
      "uuid": "00000000-0000-0000-0000-000000000002",
      "company": "Tech Solutions Inc.",
      "total_job_openings": 2,
      "title": "Ingeniero DevOps",
      "mode": "hybrid",
      "starts_at": "2025-10-15T00:00:00+00:00",
      "finishes_at": "2025-12-15T23:59:59+00:00"
    }
  ],
  "links": {
    "first": "https://sandbox.su-dominio.com/api/v1/company/opportunities?page=1",
    "last": "https://sandbox.su-dominio.com/api/v1/company/opportunities?page=5",
    "prev": null,
    "next": "https://sandbox.su-dominio.com/api/v1/company/opportunities?page=2"
  },
  "meta": {
    "current_page": 1,
    "from": 1,
    "last_page": 5,
    "path": "https://sandbox.su-dominio.com/api/v1/company/opportunities",
    "per_page": 10,
    "to": 10,
    "total": 47
  }
}
```

## Estructura JSON Explicada

| Campo                    | Tipo     | Descripción |
| ------------------------ | -------- | ----------- |
| data[]                   | array    | Lista de oportunidades de empleo. |
| data[].uuid              | string   | UUID de la oportunidad de empleo. |
| data[].company           | string   | Nombre de la empresa (solo cuando no es propietario). |
| data[].total_job_openings| integer  | Número de puestos abiertos. |
| data[].title             | string   | Título del trabajo en el idioma solicitado. |
| data[].mode              | string   | Modo de trabajo: `remote`, `hybrid`, `on-site`. |
| data[].starts_at         | datetime | Fecha de inicio de la oportunidad. |
| data[].finishes_at       | datetime | Fecha de finalización de la oportunidad. |
| links                    | object   | Enlaces de paginación (cuando está paginado). |
| links.first              | string   | URL a la primera página. |
| links.last               | string   | URL a la última página. |
| links.prev               | string   | URL a la página anterior. |
| links.next               | string   | URL a la siguiente página. |
| meta                     | object   | Metadatos de paginación (cuando está paginado). |
| meta.current_page        | integer  | Número de página actual. |
| meta.from                | integer  | Índice del ítem inicial. |
| meta.last_page           | integer  | Número total de páginas. |
| meta.path                | string   | Ruta base de URL. |
| meta.per_page            | integer  | Ítems por página. |
| meta.to                  | integer  | Índice del ítem final. |
| meta.total               | integer  | Número total de ítems. |

## Estados HTTP

- 200: OK
- 400: Solicitud inválida
- 401: No autorizado
- 404: No encontrado
- 429: Demasiadas solicitudes
- 500: Error interno del servidor

## Errores

```json
{
  "message": "Clave de plataforma inválida.",
  "errors": {}
}
```

## Notas

- Devuelve oportunidades de empleo para la empresa asociada a la plataforma actual (identificada por `X-PUBLIC-KEY`).
- Las oportunidades se ordenan por `finishes_at` en orden descendente (más reciente primero).
- Cuando `noPaginate=true`, se omiten los metadatos de paginación (`links` y `meta`).
- El campo `company` solo se incluye en la respuesta cuando la solicitud no proviene del propietario.
- Las oportunidades eliminadas se excluyen por defecto. Use `withTrash=true` para incluirlas.
- Los títulos y descripciones de trabajos se devuelven en el idioma solicitado, recurriendo al idioma predeterminado si no está disponible.

## Relacionados

- [Job Opportunity Show](./MeJobOpportunityShow.md)
- [Company Show](./CompanyShow.md)

## Changelog

- 2025-10-16: Documentación inicial.
