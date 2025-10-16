# Company – Listar Empresas por Cobertura con Geolocalizaciones

## Endpoint

```
GET /api/v1/company/through-coverage/geolocations
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
| sort_by | string | No | Campo para ordenar. | name (máx: 255) |
| order_by | string | No | Dirección de ordenamiento. | ASC (máx: 4) |
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
  "https://sandbox.su-dominio.com/api/v1/company/through-coverage/geolocations?name=Ejemplo"
```

### Ejemplo de respuesta

```json
{
  "data": [
    {
      "uuid": "123e4567-e89b-12d3-a456-426614174000",
      "name": "Empresa Ejemplo",
      "slug": "empresa-ejemplo",
      "geolocation": {
        "id": 1,
        "latitude": "-25.4284",
        "longitude": "-49.2733"
      }
    }
  ]
}
```

## Estructura JSON Explicada

| Campo       | Tipo    | Descripción |
| ----------- | ------- | ----------- |
| data[] | array | Lista de empresas con geolocalizaciones. |
| data[].uuid | string | UUID de la empresa. |
| data[].name | string | Nombre de la empresa. |
| data[].slug | string | Identificador amigable de la empresa. |
| data[].geolocation | object | Datos de geolocalización (null si no está disponible). |
| data[].geolocation.id | integer | ID de geolocalización. |
| data[].geolocation.latitude | string | Coordenada de latitud. |
| data[].geolocation.longitude | string | Coordenada de longitud. |

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

- Este endpoint devuelve solo empresas que tienen datos de geolocalización en su dirección.
- Las empresas sin datos de geolocalización se excluyen automáticamente.
- La respuesta no está paginada y devuelve todos los resultados coincidentes.
- Útil para mostrar empresas en mapas o funciones basadas en ubicación.
- Todos los parámetros de filtro del endpoint de índice principal son compatibles.

## Relacionados

- [Listar Empresas por Cobertura](CompanyThroughCoverageIndex.md)
- [Contar Empresas por Cobertura](CompanyThroughCoverageCounter.md)
- [Mostrar Empresa](CompanyShow.md)
