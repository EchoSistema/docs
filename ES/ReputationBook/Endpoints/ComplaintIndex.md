# ReputationBook – Listar Quejas Públicas

## Endpoint

```
GET /api/v1/complaints
```

Recupera una lista paginada de quejas públicas con opciones completas de filtrado.

## Autenticación

Ninguna (endpoint público)

## Encabezados

| Encabezado       | Tipo   | Requerido | Descripción |
| ---------------- | ------ | --------- | ----------- |
| X-PUBLIC-KEY     | string | Sí        | Clave pública de la plataforma. |
| Accept-Language  | string | No        | Locale IETF (ej.: `pt-BR`, `en`, `es`). Determina la localización de campos traducibles. |

## Parámetros

### Parámetros de consulta

| Parámetro | Tipo    | Requerido | Descripción | Predeterminado/Valores |
| --------- | ------- | --------- | ----------- | ---------------------- |
| user      | string  | No        | Filtrar por UUID, ID o nombre de usuario. | - |
| company   | string  | No        | Filtrar por UUID, ID, nombre, email o URL de empresa. Acepta prefijos `email:` y `url:`. | - |
| company_name | string | No | Filtrar por nombre de empresa (coincidencia parcial). | - |
| company_email | string | No | Filtrar por email de empresa. | - |
| company_url | string | No | Filtrar por URL del sitio web de la empresa. | - |
| status    | string  | No        | Filtrar por estado de queja (ej.: `pending`, `in_progress`, `resolved`, `rejected`). | - |
| is_public | boolean | No        | Filtrar por visibilidad pública. Automáticamente establecido en `true` para este endpoint. | true |
| title     | string  | No        | Filtrar por título de queja (coincidencia parcial). | - |
| complaint | string  | No        | Filtrar por texto de queja (coincidencia parcial). | - |
| complaint_language | string | No | Filtrar quejas por código de idioma del texto. | - |
| invoice   | string  | No        | Filtrar por número de factura. | - |
| created_at | date   | No        | Filtrar por fecha exacta de creación. | - |
| created_at_period | array | No | Filtrar por rango de fechas `[fecha_inicio, fecha_fin]`. | - |
| purchase_date | date | No | Filtrar por fecha exacta de compra. | - |
| purchase_date_period | array | No | Filtrar por rango de fechas de compra `[fecha_inicio, fecha_fin]`. | - |
| month     | integer | No        | Filtrar por mes (1-12). | - |
| year      | integer | No        | Filtrar por año (1990-2100). Requerido si se proporciona `month`. | - |
| with_texts | boolean | No | Incluir contenido de texto de queja en la respuesta. | false |
| per_page  | integer | No        | Ítems por página para paginación. | 15 |
| no_paginate | boolean | No | Cuando es `true`, desactiva la paginación y retorna todos los resultados (limitado por `limit`). | false |
| limit     | integer | No        | Número máximo de resultados cuando `no_paginate` es true. | - |

> Todos los parámetros de filtro aceptan variantes `camelCase`, `kebab-case` y `snake_case`.

## Ejemplos

### Ejemplo de solicitud (curl)

```bash
curl -X GET \
  -H "X-PUBLIC-KEY: <clave>" \
  -H "Accept-Language: es" \
  "https://sandbox.su-dominio.com/api/v1/complaints?status=pending&per_page=10"
```

### Ejemplo de respuesta

```json
{
  "data": [
    {
      "uuid": "9d1234ab-5678-90ef-1234-567890abcdef",
      "user": {
        "uuid": "9a1234ab-5678-90ef-1234-567890abcdef",
        "name": "Juan Pérez",
        "gender": "male"
      },
      "platform": {
        "uuid": "9b1234ab-5678-90ef-1234-567890abcdef",
        "name": "Nombre de Plataforma",
        "domain_area": {
          "uuid": "9c1234ab-5678-90ef-1234-567890abcdef",
          "name": "Área de Servicio",
          "slug": "area-servicio"
        }
      },
      "company": {
        "uuid": "9e1234ab-5678-90ef-1234-567890abcdef",
        "name": "Empresa ABC",
        "slug": "empresa-abc"
      },
      "titles": [
        {
          "content": "Problema de calidad del producto",
          "language": "es",
          "is_default": true
        }
      ],
      "status": "pending",
      "is_public": true,
      "purchase_date": "2025-10-01",
      "created_at": "2025-10-15T14:30:00.000000Z",
      "updated_at": "2025-10-15T14:30:00.000000Z"
    }
  ],
  "links": {
    "first": "https://api.example.com/api/v1/complaints?page=1",
    "last": "https://api.example.com/api/v1/complaints?page=5",
    "prev": null,
    "next": "https://api.example.com/api/v1/complaints?page=2"
  },
  "meta": {
    "current_page": 1,
    "from": 1,
    "last_page": 5,
    "path": "https://api.example.com/api/v1/complaints",
    "per_page": 15,
    "to": 15,
    "total": 72
  }
}
```

## Estructura JSON Explicada

| Campo       | Tipo    | Descripción |
| ----------- | ------- | ----------- |
| data[]      | array   | Lista de objetos de quejas. |
| data[].uuid | uuid    | Identificador único de la queja. |
| data[].user | object  | Usuario que creó la queja. |
| data[].platform | object | Plataforma donde se registró la queja. |
| data[].company | object | Empresa sobre la que se queja. |
| data[].titles[] | array | Títulos de queja localizados. |
| data[].status | string | Estado actual de la queja. |
| data[].is_public | boolean | Si la queja es visible públicamente. |
| data[].purchase_date | date | Fecha de compra relacionada con la queja. |
| data[].created_at | string | Marca de tiempo de creación de queja (ISO-8601). |
| data[].updated_at | string | Marca de tiempo de última actualización de queja (ISO-8601). |
| links       | object  | Enlaces de paginación. |
| meta        | object  | Metadatos de paginación. |

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
    "status": ["El estado seleccionado no es válido."]
  }
}
```

## Notas

- Este endpoint filtra automáticamente para mostrar solo quejas públicas (`is_public=true`).
- El array `titles` se filtra por el encabezado `Accept-Language` o retorna el título predeterminado.
- Cuando `with_texts=true`, el contenido de texto de la queja se incluye en la respuesta.
- La paginación está habilitada por defecto con 15 ítems por página.
- Los filtros de cadena realizan coincidencia parcial sin distinción entre mayúsculas y minúsculas.

## Relacionados

- [Mostrar Queja](ComplaintShow.md)
- [Crear Queja](ComplaintStore.md)
- [Estados de Quejas](ComplaintStatus.md)
