# Company – Contar Empresas por Cobertura

## Endpoint

```
GET /api/v1/company/through-coverage/counter
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

| Parámetro | Tipo    | Requerido | Descripción | Predeterminado/Valores |
| --------- | ------- | --------- | ----------- | ---------------------- |
| id | integer | No | Filtrar por ID de empresa. | - |
| uuids | array | No | Filtrar por UUIDs de empresa. | - |
| name | string | No | Filtrar por nombre de empresa (coincidencia parcial). | máx: 255 |
| slug | string | No | Filtrar por slug de empresa. | máx: 255 |
| fiscal_document_type | string | No | Tipo de documento fiscal. Requerido con `fiscal_document_value`. | valores enum |
| fiscal_document_value | string | No | Valor del documento fiscal. | máx: 255 |
| no_complaints | boolean | No | Filtrar empresas sin quejas. | false |
| has_complaints | boolean | No | Filtrar empresas con quejas. | false |
| no_reviews | boolean | No | Filtrar empresas sin reseñas. | false |
| has_reviews | boolean | No | Filtrar empresas con reseñas. | false |
| rating | numeric | No | Filtrar por calificación. | 0-5 |
| good_rating | boolean | No | Filtrar empresas con buena calificación. | false |
| bad_rating | boolean | No | Filtrar empresas con mala calificación. | false |

> Los nombres de parámetros están documentados en `snake_case`. El endpoint acepta variantes (`camelCase`, `kebab-case`, `CapitalCase`).

## Ejemplos

### Ejemplo de solicitud (curl)

```bash
curl -X GET \
  -H "X-PUBLIC-KEY: <clave>" \
  -H "Accept-Language: es" \
  "https://sandbox.su-dominio.com/api/v1/company/through-coverage/counter?has_reviews=true"
```

### Ejemplo de respuesta

```json
{
  "data": {
    "total": 42
  }
}
```

## Estructura JSON Explicada

| Campo       | Tipo    | Descripción |
| ----------- | ------- | ----------- |
| data | object | Contenedor de datos de respuesta. |
| data.total | integer | Conteo total de empresas que coinciden con los filtros. |

## Estados HTTP

- 200: OK
- 400: Solicitud inválida
- 401: No autorizado
- 422: Entidad no procesable
- 429: Demasiadas solicitudes
- 500: Error interno del servidor

## Errores

Errores de validación estándar:

```json
{
  "message": "Los datos proporcionados no son válidos.",
  "errors": {
    "rating": ["La calificación debe estar entre 0 y 5."]
  }
}
```

## Notas

- Este endpoint devuelve el conteo total de empresas que coinciden con los filtros proporcionados.
- Todos los parámetros de filtro del endpoint de índice principal son compatibles.
- El conteo respeta las restricciones del área de cobertura de la plataforma.
- Útil para mostrar estadísticas o información de paginación sin obtener datos completos.
- Más eficiente que obtener todos los resultados cuando solo se necesita el conteo.

## Relacionados

- [Listar Empresas por Cobertura](CompanyThroughCoverageIndex.md)
- [Listar Empresas por Cobertura con Geolocalizaciones](CompanyThroughCoverageGeolocations.md)
- [Mostrar Empresa](CompanyShow.md)
