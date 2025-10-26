# ReputationBook – Listar Quejas (Backoffice)

## Endpoint

```
GET /api/v1/backoffice/complaints
```

## Descripción

Lista todas las quejas del sistema con filtros avanzados. Endpoint exclusivo para administradores del backoffice.

## Autenticación

Obligatoria – Bearer {token} con habilidad `backoffice`

## Encabezados

| Encabezado       | Tipo   | Obligatorio | Descripción |
| ---------------- | ------ | ----------- | ----------- |
| Authorization    | string | Sí          | Credencial `Bearer {token}`. |
| X-PUBLIC-KEY     | string | Sí          | Clave pública de la plataforma. |
| Accept-Language  | string | No          | Locale IETF (ej.: `pt-BR`, `en`, `es`). |

## Parámetros de Query

| Parámetro      | Tipo    | Obligatorio | Descripción |
| -------------- | ------- | ----------- | ----------- |
| platform_id    | integer | No          | Filtrar por ID de plataforma |
| company_id     | integer | No          | Filtrar por ID de empresa |
| user_id        | integer | No          | Filtrar por ID de usuario |
| status         | string  | No          | Filtrar por estado de la queja |
| is_public      | boolean | No          | Filtrar por visibilidad pública |
| language       | string  | No          | Idioma de los títulos (predeterminado: pt-BR) |
| no_paginate    | boolean | No          | Si es `true`, devuelve todos los resultados sin paginación |
| per_page       | integer | No          | Registros por página (predeterminado: 15) |
| limit          | integer | No          | Límite de resultados cuando `no_paginate=true` |

## Ejemplo de Respuesta

```json
{
  "data": [
    {
      "uuid": "9d4e1c2a-3b4c-5d6e-7f8g-9h0i1j2k3l4m",
      "is_public": true,
      "status": "pending",
      "purchase_date": "2024-01-10T00:00:00Z",
      "invoice_number": "INV-2024-001",
      "created_at": "2024-01-15T10:30:00Z",
      "user": {
        "uuid": "8c3d0b1a-2e3f-4g5h-6i7j-8k9l0m1n2o3p",
        "name": "Juan Pérez",
        "email": "juan.perez@example.com",
        "gender": "Masculino"
      },
      "platform": {
        "uuid": "7b2c9a0b-1d2e-3f4g-5h6i-7j8k9l0m1n2o",
        "name": "Plataforma Demo",
        "domain_area": {
          "uuid": "6a1b8c9d-0e1f-2g3h-4i5j-6k7l8m9n0o1p",
          "name": "E-commerce",
          "slug": "ecommerce"
        }
      },
      "title": {
        "title": "Producto defectuoso",
        "slug": "producto-defectuoso",
        "language": "es",
        "is_default": true
      }
    }
  ]
}
```

## Relaciones Cargadas

- `user` - Usuario que creó la queja
- `platform` - Plataforma donde se registró
- `platform.domainArea` - Área de dominio de la plataforma
- `company` - Empresa reclamada (cuando disponible)
- `titles` - Títulos multilingües

## Filtros Disponibles

- **platform_id**: Filtrar por plataforma específica
- **company_id**: Filtrar por empresa específica
- **user_id**: Filtrar por usuario específico
- **status**: Filtrar por estado
- **is_public**: Filtrar por visibilidad pública
- **language**: Define el idioma de los títulos devueltos

## Estado HTTP

- 200: OK
- 401: No autenticado
- 403: Prohibido (sin habilidad `backoffice`)
- 422: Error de validación
- 429: Demasiadas solicitudes
- 500: Error interno

## Notas

- Requiere habilidad `backoffice` en el token de autenticación
- Ordenación predeterminada: más recientes primero
- La paginación predeterminada devuelve 15 registros por página
- Use `no_paginate=true` con `limit` para obtener todos los resultados
- Todos los correos electrónicos y datos sensibles están expuestos (endpoint administrativo)
- Las fechas siguen el formato ISO 8601

## Relacionados

- [Ver Detalles de la Queja](BackofficeComplaintShow.md)

## Changelog

- 2025-10-26: Documentación inicial del endpoint de backoffice
