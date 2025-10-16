# RealEstate – Listar Propiedades

## Endpoint

```
GET /api/v1/real-estate/properties
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
| uuid | array | No | Filtrar por UUIDs de propiedades específicas | - |
| language | string | No | Código de idioma para campos traducibles | - |
| city | array | No | Filtrar por nombre(s) de ciudad | - |
| country | array | No | Filtrar por código(s) de país (ISO 3166-1 alfa-2) | - |
| region | array | No | Filtrar por nombre(s) de región | - |
| rent_type | array | No | Filtrar por tipo(s) de alquiler | - |
| property_type | array | No | Filtrar por tipo(s) de propiedad | - |
| agency_uuid | array | No | Filtrar por UUID(s) de agencia | - |
| agency | array | No | Filtrar por nombre(s) de agencia | - |
| agent_uuid | array | No | Filtrar por UUID(s) de agente | - |
| agent | array | No | Filtrar por nombre(s) de agente | - |
| rooms | integer | No | Filtrar por número exacto de habitaciones | - |
| rooms_min | integer | No | Filtrar por número mínimo de habitaciones | - |
| rooms_max | integer | No | Filtrar por número máximo de habitaciones | - |
| bed | integer | No | Filtrar por número exacto de dormitorios | - |
| bed_min | integer | No | Filtrar por número mínimo de dormitorios | - |
| bed_max | integer | No | Filtrar por número máximo de dormitorios | - |
| bath | integer | No | Filtrar por número exacto de baños | - |
| bath_min | integer | No | Filtrar por número mínimo de baños | - |
| bath_max | integer | No | Filtrar por número máximo de baños | - |
| size_min | integer | No | Filtrar por tamaño mínimo (m²) | - |
| size_max | integer | No | Filtrar por tamaño máximo (m²) | - |
| is_featured | boolean | No | Filtrar propiedades destacadas | - |
| direct_from_owner | boolean | No | Filtrar propiedades directamente del propietario | - |
| search | string | No | Búsqueda de texto completo en campos de propiedad | - |
| value_currency | string | No | Código de moneda (ISO 4217, 3 caracteres) | - |
| currency | string | No | Parámetro alternativo de moneda | - |
| value_min | numeric | No | Filtrar por precio mínimo | - |
| value_max | numeric | No | Filtrar por precio máximo | - |
| created_at | date | No | Filtrar por fecha de creación | - |
| created_at_period | object | No | Filtrar por rango de fechas (from/to o start/end) | - |
| sort_by | string | No | Campo por el cual ordenar | `createdAt` |
| direction | string | No | Dirección de ordenación | `DESC` |
| per_page | integer | No | Elementos por página | 9 (máx: 200) |
| page | integer | No | Número de página | 1 |

> Los parámetros aceptan variantes: `snake_case`, `camelCase`, `kebab-case`, `CapitalCase`.

**Campos ordenables:** `created_at`, `createdat`, `created`, `createdAt`, `size`, `rooms`, `bed`, `beds`, `bedrooms`, `bath`, `baths`, `bathrooms`, `garage`, `is_featured`, `isFeatured`, `direct_from_owner`, `directFromOwner`, `value`, `price`.

## Ejemplos

### Ejemplo de solicitud (curl)

```bash
curl -X GET \
  -H "X-PUBLIC-KEY: <clave>" \
  -H "Accept-Language: es" \
  "https://sandbox.su-dominio.com/api/v1/real-estate/properties?per_page=9&city[]=Miami&bed_min=2&value_max=500000"
```

### Ejemplo de respuesta

```json
{
  "data": [
    {
      "uuid": "550e8400-e29b-41d4-a716-446655440000",
      "title": "Apartamento Moderno de 3 Dormitorios",
      "description": "Hermoso apartamento con vista al mar",
      "property_type": "Apartamento",
      "rent_type": "Venta",
      "value": 450000,
      "currency": "USD",
      "size": 120,
      "rooms": 3,
      "bedrooms": 3,
      "bathrooms": 2,
      "garage": 1,
      "is_featured": true,
      "direct_from_owner": false,
      "address": {
        "city": "Miami",
        "region": "Florida",
        "country": "US"
      },
      "agent": {
        "uuid": "660e8400-e29b-41d4-a716-446655440001",
        "name": "John Smith"
      },
      "images": [
        {
          "url": "https://example.com/property1.jpg",
          "is_primary": true
        }
      ],
      "created_at": "2025-10-16T10:30:00Z"
    }
  ],
  "links": {
    "first": "https://sandbox.su-dominio.com/api/v1/real-estate/properties?page=1",
    "last": "https://sandbox.su-dominio.com/api/v1/real-estate/properties?page=5",
    "prev": null,
    "next": "https://sandbox.su-dominio.com/api/v1/real-estate/properties?page=2"
  },
  "meta": {
    "current_page": 1,
    "from": 1,
    "last_page": 5,
    "per_page": 9,
    "to": 9,
    "total": 42
  }
}
```

## Estructura JSON Explicada

| Campo       | Tipo    | Descripción |
| ----------- | ------- | ----------- |
| data[] | array | Lista de propiedades |
| data[].uuid | string | Identificador único de la propiedad |
| data[].title | string | Título de la propiedad |
| data[].description | string | Descripción de la propiedad |
| data[].property_type | string | Tipo de propiedad (Apartamento, Casa, etc.) |
| data[].rent_type | string | Tipo de transacción (Venta, Alquiler, etc.) |
| data[].value | numeric | Precio/valor de la propiedad |
| data[].currency | string | Código de moneda |
| data[].size | integer | Tamaño de la propiedad en metros cuadrados |
| data[].rooms | integer | Número total de habitaciones |
| data[].bedrooms | integer | Número de dormitorios |
| data[].bathrooms | integer | Número de baños |
| data[].garage | integer | Número de espacios de garaje |
| data[].is_featured | boolean | Si la propiedad está destacada |
| data[].direct_from_owner | boolean | Si la propiedad es directamente del propietario |
| data[].address | object | Información de la dirección de la propiedad |
| data[].agent | object | Información del agente |
| data[].images[] | array | Imágenes de la propiedad |
| data[].created_at | string | Marca de tiempo de creación de la propiedad (ISO 8601) |
| links | object | Enlaces de paginación |
| meta | object | Metadatos de paginación |

## Estados HTTP

- 200: OK
- 400: Solicitud inválida
- 401: No autorizado
- 422: Entidad no procesable
- 429: Demasiadas solicitudes
- 500: Error interno del servidor

## Errores

```json
{
  "message": "Error de validación",
  "errors": {
    "per_page": ["El campo per page no debe ser mayor que 200."],
    "country": ["El campo country debe tener 2 caracteres."]
  }
}
```

## Notas

- El parámetro `public_key` se extrae automáticamente del encabezado `X-PUBLIC-KEY`
- Los resultados están paginados con un predeterminado de 9 elementos por página (máx 200)
- Los parámetros de array (city, country, etc.) pueden pasarse múltiples veces o como notación de array
- Los filtros de fecha soportan varios formatos: fecha única o período con from/to o start/end
- Todos los campos traducibles respetan el encabezado `Accept-Language`
- La ordenación predeterminada es por fecha de creación en orden descendente (más reciente primero)

## Relacionados

- [Mostrar Detalles de Propiedad](PropertyShow.md)
- [Listar Tipos de Propiedad](PropertyTypeIndex.md)
- [Listar Tipos de Alquiler](RentTypeIndex.md)
- [Listar Agentes](AgentIndex.md)

## Changelog

- 2025-10-16: Documentación inicial
