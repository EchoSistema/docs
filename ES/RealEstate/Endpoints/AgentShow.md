# RealEstate – Mostrar Detalles de Agente

## Endpoint

```
GET /api/v1/real-estate/agents/{uuid}
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
| uuid      | string | Sí        | Identificador único del agente (formato UUID) |

## Ejemplos

### Ejemplo de solicitud (curl)

```bash
curl -X GET \
  -H "X-PUBLIC-KEY: <clave>" \
  -H "Accept-Language: es" \
  "https://sandbox.su-dominio.com/api/v1/real-estate/agents/660e8400-e29b-41d4-a716-446655440001"
```

### Ejemplo de respuesta

```json
{
  "data": {
    "uuid": "660e8400-e29b-41d4-a716-446655440001",
    "name": "John Smith",
    "email": "john.smith@agency.com",
    "phone": "+1-555-0123",
    "mobile": "+1-555-0124",
    "bio": "Agente inmobiliario experimentado con más de 10 años en el mercado de Miami. Especializado en propiedades de lujo y casas frente al mar.",
    "agency": {
      "uuid": "770e8400-e29b-41d4-a716-446655440002",
      "name": "Premium Real Estate",
      "address": "123 Main Street, Miami, FL",
      "website": "https://premiumrealestate.com"
    },
    "avatar": "https://example.com/agents/john-smith.jpg",
    "properties_count": 25,
    "specialties": [
      "Propiedades de Lujo",
      "Casas Frente al Mar",
      "Bienes Raíces Comerciales"
    ],
    "languages": ["Inglés", "Español"],
    "social_media": {
      "facebook": "https://facebook.com/johnsmith",
      "instagram": "https://instagram.com/johnsmith",
      "linkedin": "https://linkedin.com/in/johnsmith"
    },
    "created_at": "2024-01-15T08:00:00Z",
    "updated_at": "2025-10-16T10:30:00Z"
  }
}
```

## Estructura JSON Explicada

| Campo       | Tipo    | Descripción |
| ----------- | ------- | ----------- |
| data | object | Detalles del agente |
| data.uuid | string | Identificador único del agente |
| data.name | string | Nombre completo del agente |
| data.email | string | Dirección de correo electrónico del agente |
| data.phone | string | Número de teléfono de oficina del agente |
| data.mobile | string | Número de teléfono móvil del agente |
| data.bio | string | Biografía/descripción del agente |
| data.agency | object | Información de la agencia |
| data.agency.uuid | string | Identificador único de la agencia |
| data.agency.name | string | Nombre de la agencia |
| data.agency.address | string | Dirección física de la agencia |
| data.agency.website | string | URL del sitio web de la agencia |
| data.avatar | string | URL de la foto de perfil del agente |
| data.properties_count | integer | Número de propiedades activas gestionadas por el agente |
| data.specialties[] | array | Lista de especialidades del agente |
| data.languages[] | array | Idiomas hablados por el agente |
| data.social_media | object | Enlaces de perfiles de redes sociales |
| data.created_at | string | Marca de tiempo de registro del agente (ISO 8601) |
| data.updated_at | string | Marca de tiempo de última actualización del agente (ISO 8601) |

## Estados HTTP

- 200: OK
- 404: No encontrado
- 429: Demasiadas solicitudes
- 500: Error interno del servidor

## Errores

```json
{
  "message": "Agente no encontrado"
}
```

## Notas

- El agente debe pertenecer a la plataforma especificada por el encabezado `X-PUBLIC-KEY`
- Todos los campos traducibles respetan el encabezado `Accept-Language`
- Los enlaces de redes sociales son opcionales y pueden ser nulos
- El `properties_count` incluye solo propiedades activas/publicadas

## Relacionados

- [Listar Agentes](AgentIndex.md)
- [Listar Propiedades](PropertyIndex.md)

## Changelog

- 2025-10-16: Documentación inicial
