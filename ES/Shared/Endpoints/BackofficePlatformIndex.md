# Shared – Listar Plataformas

## Endpoint

```
GET /api/v1/backoffice/platforms
```

## Autenticación

Requerida – Bearer {token} con habilidad `backoffice`

## Encabezados

| Encabezado     | Tipo | Requerido | Descripción |
| ---------------- | ------ | -------- | ----------- |
| Authorization    | string | Cuando aplica | Credencial `Bearer {token}`. |
| X-PUBLIC-KEY     | string | Sí      | Clave pública de la plataforma. |
| Accept-Language  | string | No       | Locale IETF (ej.: `pt-BR`, `en`, `es`). |

## Parámetros de Query

| Parámetro | Tipo | Requerido | Descripción |
| ----------- | ------- | ----------- | ----------- |
| no_paginate | boolean | No          | Si es `true`, retorna todos los resultados sin paginación. |
| per_page    | integer | No          | Número de registros por página (por defecto: 25). |

## Ejemplos

### Ejemplo de solicitud (curl)

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: es" \
  "https://sandbox.your-domain.com/api/v1/backoffice/platforms?per_page=25"
```

### Ejemplo de respuesta

```json
{
  "data": [
    {
      "uuid": "9d4e1c2a-3b4c-5d6e-7f8g-9h0i1j2k3l4m",
      "total": {
        "users": 150
      },
      "is_parent": true,
      "domain": {
        "uuid": "8c3d0b1a-2e3f-4g5h-6i7j-8k9l0m1n2o3p",
        "slug": "ecommerce",
        "name": "E-commerce"
      },
      "name": "Plataforma Demo",
      "email": "contacto@plataforma.com",
      "open_to_register": true,
      "public_key": "pk_test_123456789",
      "logo": {
        "main": "https://cdn.example.com/logos/main.png",
        "square": "https://cdn.example.com/logos/square.png",
        "rounded": "https://cdn.example.com/logos/rounded.png",
        "rectangular": "https://cdn.example.com/logos/rectangular.png"
      },
      "created_at": "2024-01-15T10:30:00Z",
      "company": {
        "uuid": "7b2c9a0b-1d2e-3f4g-5h6i-7j8k9l0m1n2o",
        "name": "Empresa Demo Ltda",
        "slug": "empresa-demo",
        "zipcode": "01310-100",
        "country": "Brasil",
        "state": "São Paulo",
        "city": "São Paulo",
        "company_logos": [
          {
            "main_logo": "https://cdn.example.com/company/main.png"
          },
          {
            "square_logo": "https://cdn.example.com/company/square.png"
          }
        ],
        "full_address": "Av. Paulista, 1000 - Bela Vista, São Paulo - SP, 01310-100",
        "links": [
          {
            "uuid": "6a1b8c9d-0e1f-2g3h-4i5j-6k7l8m9n0o1p",
            "platform": "Facebook",
            "url": "https://facebook.com/plataforma"
          },
          {
            "uuid": "5a0b7c8d-9e0f-1g2h-3i4j-5k6l7m8n9o0p",
            "platform": "Instagram",
            "url": "https://instagram.com/plataforma"
          }
        ]
      }
    }
  ],
  "links": {
    "first": "https://sandbox.your-domain.com/api/v1/backoffice/platforms?page=1",
    "last": "https://sandbox.your-domain.com/api/v1/backoffice/platforms?page=5",
    "prev": null,
    "next": "https://sandbox.your-domain.com/api/v1/backoffice/platforms?page=2"
  },
  "meta": {
    "current_page": 1,
    "from": 1,
    "last_page": 5,
    "per_page": 25,
    "to": 25,
    "total": 125
  }
}
```

## Estructura JSON Explicada

### Campos Principales

| Campo | Tipo | Descripción |
| ----------- | ------- | ----------- |
| data        | array   | Lista de plataformas |
| data[].uuid | string  | Identificador único de la plataforma |
| data[].total | object  | Totalizadores de la plataforma (condicional - solo si hay usuarios) |
| data[].total.users | integer | Total de usuarios en la plataforma |
| data[].is_parent | boolean | Indica si la plataforma es una plataforma padre |
| data[].domain | object  | Información del área de dominio |
| data[].domain.uuid | string  | Identificador único del área de dominio |
| data[].domain.slug | string  | Slug del área de dominio |
| data[].domain.name | string  | Nombre del área de dominio |
| data[].name | string  | Nombre de la plataforma |
| data[].email | string  | Email de contacto de la plataforma |
| data[].open_to_register | boolean | Indica si la plataforma está abierta para nuevos registros |
| data[].public_key | string  | Clave pública de la plataforma |
| data[].logo | object  | URLs de los logos de la plataforma |
| data[].logo.main | string  | URL del logo principal |
| data[].logo.square | string  | URL del logo cuadrado |
| data[].logo.rounded | string  | URL del logo redondeado |
| data[].logo.rectangular | string  | URL del logo rectangular |
| data[].created_at | string  | Fecha de creación (ISO 8601) |

### Campos de la Empresa (condicional)

Los campos a continuación están agrupados dentro del objeto `company` y solo aparecen cuando la plataforma tiene una empresa asociada:

| Campo | Tipo | Descripción |
| ----------- | ------- | ----------- |
| data[].company | object  | Datos de la empresa (condicional) |
| data[].company.uuid | string  | Identificador único de la empresa |
| data[].company.name | string  | Nombre de la empresa |
| data[].company.slug | string  | Slug de la empresa |
| data[].company.zipcode | string\|null  | Código postal de la empresa |
| data[].company.country | string\|null  | País de la empresa |
| data[].company.state | string\|null  | Estado de la empresa |
| data[].company.city | string\|null  | Ciudad de la empresa |
| data[].company.company_logos | array   | Logos de la empresa (array vacío si no está cargado) |
| data[].company.full_address | string\|null  | Dirección completa formateada |
| data[].company.links | array   | Enlaces de redes sociales de la empresa (array vacío si no está cargado) |
| data[].company.links[].uuid | string  | Identificador único del enlace |
| data[].company.links[].platform | string  | Nombre de la plataforma social |
| data[].company.links[].url | string  | URL del perfil social |

### Metadatos de Paginación

| Campo | Tipo | Descripción |
| ----------- | ------- | ----------- |
| links       | object  | Enlaces de navegación entre páginas |
| links.first | string  | URL de la primera página |
| links.last | string  | URL de la última página |
| links.prev | string\|null  | URL de la página anterior (null si es la primera página) |
| links.next | string\|null  | URL de la próxima página (null si es la última página) |
| meta        | object  | Metadatos de la paginación |
| meta.current_page | integer | Número de la página actual |
| meta.from | integer | Número del primer registro de la página |
| meta.last_page | integer | Número de la última página |
| meta.per_page | integer | Registros por página |
| meta.to | integer | Número del último registro de la página |
| meta.total | integer | Total de registros |

## Relaciones Cargadas

Este endpoint carga automáticamente las siguientes relaciones:

- `domainArea` - Área de dominio de la plataforma
- `company` - Empresa asociada
- `company.logos` - Logos de la empresa
- `company.address` - Dirección de la empresa (usado para country, state, city)
- `company.socialMedias` - Redes sociales de la empresa

## Contadores

- `users_count` - Total de usuarios registrados en la plataforma

## Estado HTTP

- 200: OK
- 401: No autenticado
- 403: Prohibido (sin permiso `index.all`)
- 422: Error de validación
- 429: Límite de solicitudes excedido
- 500: Error interno

## Errores

```json
{
  "message": "Mensaje de error"
}
```

## Notas

- Este endpoint retorna todas las plataformas registradas en el sistema
- Solo usuarios con permiso `index.all` pueden acceder
- Ordenado por fecha de creación (descendente) - las más recientes aparecen primero
- El campo `total.users` solo aparece cuando la plataforma tiene usuarios registrados
- El objeto `company` solo aparece cuando la plataforma tiene una empresa asociada
- Los arrays `company_logos` y `links` retornan array vacío cuando las relaciones no están cargadas
- Todas las URLs de logo son completas y listas para usar
- Las fechas siguen el formato ISO 8601
- La paginación por defecto retorna 25 registros por página
- Use `no_paginate=true` para obtener todos los resultados sin paginación

## Relacionados

- [Mostrar Detalles de la Plataforma](BackofficePlatformShow.md)
- [Listar Usuarios de la Plataforma](BackofficePlatformUserIndex.md)

## Changelog

- 2025-10-25: Actualización completa con nueva estructura de company y campos adicionales
- 2025-10-16: Documentación inicial
