# RealEstate – Listar Agentes Inmobiliarios

## Endpoint

```
GET /api/v1/real-estate/agents
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
| per_page | integer | No | Elementos por página | 9 (máx: 200) |
| page | integer | No | Número de página | 1 |

## Ejemplos

### Ejemplo de solicitud (curl)

```bash
curl -X GET \
  -H "X-PUBLIC-KEY: <clave>" \
  -H "Accept-Language: es" \
  "https://sandbox.su-dominio.com/api/v1/real-estate/agents?per_page=9"
```

### Ejemplo de respuesta

```json
{
  "data": [
    {
      "uuid": "660e8400-e29b-41d4-a716-446655440001",
      "name": "John Smith",
      "email": "john.smith@agency.com",
      "phone": "+1-555-0123",
      "bio": "Agente inmobiliario experimentado con más de 10 años en el mercado de Miami",
      "agency": {
        "uuid": "770e8400-e29b-41d4-a716-446655440002",
        "name": "Premium Real Estate"
      },
      "avatar": "https://example.com/agents/john-smith.jpg",
      "properties_count": 25,
      "created_at": "2024-01-15T08:00:00Z"
    }
  ],
  "links": {
    "first": "https://sandbox.su-dominio.com/api/v1/real-estate/agents?page=1",
    "last": "https://sandbox.su-dominio.com/api/v1/real-estate/agents?page=3",
    "prev": null,
    "next": "https://sandbox.su-dominio.com/api/v1/real-estate/agents?page=2"
  },
  "meta": {
    "current_page": 1,
    "from": 1,
    "last_page": 3,
    "per_page": 9,
    "to": 9,
    "total": 25
  }
}
```

## Estructura JSON Explicada

| Campo       | Tipo    | Descripción |
| ----------- | ------- | ----------- |
| data[] | array | Lista de agentes inmobiliarios |
| data[].uuid | string | Identificador único del agente |
| data[].name | string | Nombre completo del agente |
| data[].email | string | Dirección de correo electrónico del agente |
| data[].phone | string | Número de teléfono del agente |
| data[].bio | string | Biografía/descripción del agente |
| data[].agency | object | Información de la agencia |
| data[].agency.uuid | string | Identificador único de la agencia |
| data[].agency.name | string | Nombre de la agencia |
| data[].avatar | string | URL de la foto de perfil del agente |
| data[].properties_count | integer | Número de propiedades gestionadas por el agente |
| data[].created_at | string | Marca de tiempo de registro del agente (ISO 8601) |
| links | object | Enlaces de paginación |
| meta | object | Metadatos de paginación |

## Estados HTTP

- 200: OK
- 400: Solicitud inválida
- 422: Entidad no procesable
- 429: Demasiadas solicitudes
- 500: Error interno del servidor

## Errores

```json
{
  "message": "Error de validación",
  "errors": {
    "per_page": ["El campo per page no debe ser mayor que 200."]
  }
}
```

## Notas

- El parámetro `public_key` se extrae automáticamente del encabezado `X-PUBLIC-KEY`
- Los resultados están paginados con un predeterminado de 9 elementos por página (máx 200)
- Solo se devuelven agentes asociados con la plataforma
- El campo `properties_count` muestra las propiedades activas de cada agente

## Relacionados

- [Mostrar Detalles de Agente](AgentShow.md)
- [Listar Propiedades](PropertyIndex.md)

## Changelog

- 2025-10-16: Documentación inicial
