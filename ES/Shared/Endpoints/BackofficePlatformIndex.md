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
      "domain": {
        "uuid": "8c3d0b1a-2e3f-4g5h-6i7j-8k9l0m1n2o3p",
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
      "company_uuid": "7b2c9a0b-1d2e-3f4g-5h6i-7j8k9l0m1n2o",
      "slug": "plataforma-demo",
      "country": "España",
      "state": "Madrid",
      "city": "Madrid",
      "company_logos": [
        {
          "main": "https://cdn.example.com/company/main.png"
        },
        {
          "square": "https://cdn.example.com/company/square.png"
        }
      ],
      "full_address": "Calle Gran Vía, 1, 28013 Madrid, España",
      "links": [
        {
          "uuid": "6a1b8c9d-0e1f-2g3h-4i5j-6k7l8m9n0o1p",
          "platform": "facebook",
          "url": "https://facebook.com/plataforma"
        },
        {
          "uuid": "5a0b7c8d-9e0f-1g2h-3i4j-5k6l7m8n9o0p",
          "platform": "instagram",
          "url": "https://instagram.com/plataforma"
        }
      ]
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

| Campo | Tipo | Descripción |
| ----------- | ------- | ----------- |
| data        | array   | Lista de plataformas |
| data[].uuid | string  | Identificador único de la plataforma |
| data[].total | object  | Totalizadores de la plataforma |
| data[].total.users | integer | Total de usuarios en la plataforma |
| data[].domain | object  | Información del área de dominio |
| data[].domain.uuid | string  | Identificador único del área de dominio |
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
| data[].company_uuid | string  | Identificador único de la empresa (cuando está disponible) |
| data[].slug | string  | Slug de la empresa (cuando está disponible) |
| data[].country | string  | País de la empresa (cuando está disponible) |
| data[].state | string  | Estado/Provincia de la empresa (cuando está disponible) |
| data[].city | string  | Ciudad de la empresa (cuando está disponible) |
| data[].company_logos | array   | Logos de la empresa (cuando está disponible) |
| data[].full_address | string  | Dirección completa formateada (cuando está disponible) |
| data[].links | array   | Enlaces de redes sociales de la empresa (cuando está disponible) |
| data[].links[].uuid | string  | Identificador único del enlace |
| data[].links[].platform | string  | Nombre de la plataforma social |
| data[].links[].url | string  | URL del perfil social |
| links       | object  | Enlaces de paginación |
| meta        | object  | Metadatos de paginación |

## Estados HTTP

- 200: OK
- 201: Creado
- 400: Solicitud inválida
- 401: No autorizado
- 403: Prohibido
- 404: No encontrado
- 422: Entidad no procesable
- 429: Demasiadas solicitudes
- 500: Error interno del servidor

## Errores

```json
{
  "message": "Mensaje de error"
}
```

## Notas

- Este endpoint retorna todas las plataformas registradas en el sistema
- Solo usuarios con permiso de backoffice pueden acceder
- La respuesta incluye relaciones con company, company.logos y company.address
- Incluye conteo de usuarios por plataforma (users_count)
- Ordenado por fecha de creación (descendente)

## Relacionados

- [Mostrar Detalles de la Plataforma](BackofficePlatformShow.md)
- [Listar Usuarios de la Plataforma](BackofficePlatformUserIndex.md)

## Changelog

- 2025-10-25: Actualización completa de la documentación con estructura detallada
- 2025-10-16: Documentación inicial
