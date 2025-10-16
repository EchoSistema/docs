# RealEstate – Mostrar Detalles de Propiedad

## Endpoint

```
GET /api/v1/real-estate/properties/{uuid}
```

## Autenticación

Ninguna

## Encabezados

| Encabezado       | Tipo   | Requerido | Descripción |
| ---------------- | ------ | --------- | ----------- |
| X-PUBLIC-KEY     | string | Sí        | Clave pública de la plataforma. |
| Accept-Language  | string | No        | Locale IETF (ej.: `pt-BR`, `en`, `es`). |

## Parámetros

### Parámetros de ruta

| Parámetro | Tipo   | Requerido | Descripción |
| --------- | ------ | --------- | ----------- |
| uuid      | string | Sí        | Identificador único de la propiedad (formato UUID) |

## Ejemplos

### Ejemplo de solicitud (curl)

```bash
curl -X GET \
  -H "X-PUBLIC-KEY: <clave>" \
  -H "Accept-Language: es" \
  "https://sandbox.su-dominio.com/api/v1/real-estate/properties/550e8400-e29b-41d4-a716-446655440000"
```

### Ejemplo de respuesta

```json
{
  "data": {
    "uuid": "550e8400-e29b-41d4-a716-446655440000",
    "title": "Apartamento Moderno de 3 Dormitorios",
    "description": "Hermoso apartamento con vista al mar ubicado en el corazón de Miami. Esta propiedad impresionante cuenta con acabados modernos, planta abierta y vistas impresionantes del Océano Atlántico.",
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
      "street": "1234 Ocean Drive",
      "city": "Miami",
      "region": "Florida",
      "country": "US",
      "postal_code": "33139",
      "latitude": 25.7617,
      "longitude": -80.1918
    },
    "agent": {
      "uuid": "660e8400-e29b-41d4-a716-446655440001",
      "name": "John Smith",
      "email": "john.smith@agency.com",
      "phone": "+1-555-0123",
      "agency": "Premium Real Estate"
    },
    "agency": {
      "uuid": "770e8400-e29b-41d4-a716-446655440002",
      "name": "Premium Real Estate",
      "website": "https://premiumrealestate.com"
    },
    "images": [
      {
        "uuid": "880e8400-e29b-41d4-a716-446655440003",
        "url": "https://example.com/property1.jpg",
        "is_primary": true,
        "order": 1
      },
      {
        "uuid": "880e8400-e29b-41d4-a716-446655440004",
        "url": "https://example.com/property2.jpg",
        "is_primary": false,
        "order": 2
      }
    ],
    "amenities": [
      "Aire Acondicionado",
      "Piscina",
      "Gimnasio",
      "Seguridad 24/7",
      "Estacionamiento"
    ],
    "created_at": "2025-10-16T10:30:00Z",
    "updated_at": "2025-10-16T15:45:00Z"
  }
}
```

## Estructura JSON Explicada

| Campo       | Tipo    | Descripción |
| ----------- | ------- | ----------- |
| data | object | Detalles de la propiedad |
| data.uuid | string | Identificador único de la propiedad |
| data.title | string | Título de la propiedad |
| data.description | string | Descripción detallada de la propiedad |
| data.property_type | string | Tipo de propiedad (Apartamento, Casa, etc.) |
| data.rent_type | string | Tipo de transacción (Venta, Alquiler, etc.) |
| data.value | numeric | Precio/valor de la propiedad |
| data.currency | string | Código de moneda (ISO 4217) |
| data.size | integer | Tamaño de la propiedad en metros cuadrados |
| data.rooms | integer | Número total de habitaciones |
| data.bedrooms | integer | Número de dormitorios |
| data.bathrooms | integer | Número de baños |
| data.garage | integer | Número de espacios de garaje |
| data.is_featured | boolean | Si la propiedad está destacada |
| data.direct_from_owner | boolean | Si la propiedad es directamente del propietario |
| data.address | object | Información completa de la dirección |
| data.address.street | string | Dirección de la calle |
| data.address.city | string | Nombre de la ciudad |
| data.address.region | string | Nombre de la región/estado |
| data.address.country | string | Código de país (ISO 3166-1 alfa-2) |
| data.address.postal_code | string | Código postal |
| data.address.latitude | numeric | Latitud geográfica |
| data.address.longitude | numeric | Longitud geográfica |
| data.agent | object | Información del agente |
| data.agent.uuid | string | Identificador único del agente |
| data.agent.name | string | Nombre completo del agente |
| data.agent.email | string | Dirección de correo electrónico del agente |
| data.agent.phone | string | Número de teléfono del agente |
| data.agent.agency | string | Nombre de la agencia |
| data.agency | object | Información de la agencia |
| data.agency.uuid | string | Identificador único de la agencia |
| data.agency.name | string | Nombre de la agencia |
| data.agency.website | string | URL del sitio web de la agencia |
| data.images[] | array | Imágenes de la propiedad |
| data.images[].uuid | string | Identificador único de la imagen |
| data.images[].url | string | URL de la imagen |
| data.images[].is_primary | boolean | Si la imagen es la foto principal |
| data.images[].order | integer | Orden de visualización de la imagen |
| data.amenities[] | array | Lista de comodidades de la propiedad |
| data.created_at | string | Marca de tiempo de creación de la propiedad (ISO 8601) |
| data.updated_at | string | Marca de tiempo de última actualización de la propiedad (ISO 8601) |

## Estados HTTP

- 200: OK
- 404: No encontrado
- 429: Demasiadas solicitudes
- 500: Error interno del servidor

## Errores

```json
{
  "message": "Propiedad no encontrada"
}
```

## Notas

- La propiedad debe pertenecer a la plataforma especificada por el encabezado `X-PUBLIC-KEY`
- Todos los campos traducibles respetan el encabezado `Accept-Language`
- Las coordenadas geográficas pueden usarse para integración de mapas
- Las imágenes se devuelven ordenadas por el campo `order`
- La imagen principal (is_primary: true) debe mostrarse como la foto principal de la propiedad

## Relacionados

- [Listar Propiedades](PropertyIndex.md)
- [Listar Tipos de Propiedad](PropertyTypeIndex.md)
- [Mostrar Detalles de Agente](AgentShow.md)

## Changelog

- 2025-10-16: Documentación inicial
