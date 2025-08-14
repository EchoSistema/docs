# Company – Listar Empresas por Cobertura

## Endpoint

`GET /api/v1/company/through-coverage`

Recupera las empresas disponibles para la cobertura de la plataforma actual. Soporta filtrado, ordenación y paginación.

---

## Autenticación

Ninguna.

---

## Solicitud

### Encabezados Requeridos

| Encabezado | Tipo | Descripción |
| ---------- | ---- | ----------- |
| **X-PUBLIC-KEY** | `string` | Clave pública de la plataforma necesaria para autenticarse y recibir respuestas. |
| **Accept-Language** | `string` | Idioma para campos traducibles (ej.: `en`, `pt-BR`). Opcional. |

### Parámetros de Filtro

| Parámetro | Tipo | Requerido | Descripción |
| --------- | ---- | --------- | ----------- |
| `id` | `integer` | No | ID de la empresa. |
| `uuids` | `array[uuid]` | No | Filtra por UUIDs específicos de empresas. |
| `name` | `string` | No | Nombre de la empresa. |
| `slug` | `string` | No | Slug de la empresa. |
| `fiscal_document_type` | `FiscalDocumentTypeEnum` | No | Tipo de documento fiscal. |
| `fiscal_document_value` | `string` | No | Valor del documento fiscal. |
| `relations` | `array` | No | Incluye relaciones adicionales: `logos`, `contacts`, `fiscalDocuments`, `address`. |
| `no_paginate` | `boolean` | No | Deshabilita la paginación. Acepta `true`, `false`, `0` o `1`. |
| `per_page` | `integer` | No | Ítems por página cuando paginado. |
| `page` | `integer` | No | Número de página cuando paginado. |
| `sort_by` | `string` | No | Columna para ordenar. |
| `order_by` | `string` | No | Dirección de la ordenación (`ASC` o `DESC`). |
| `with_count` | `array` | No | Relaciones para contar (ej.: `complaints`, `reviews`). |
| `with_exists` | `array` | No | Relaciones para verificar existencia (ej.: `complaints`, `reviews`). |
| `no_complaints` | `boolean` | No | Empresas sin reclamaciones. |
| `has_complaints` | `boolean` | No | Empresas con reclamaciones. |
| `no_reviews` | `boolean` | No | Empresas sin reseñas. |
| `has_reviews` | `boolean` | No | Empresas con reseñas. |
| `rating` | `float` | No | Filtro por promedio de calificación. |
| `good_rating` | `boolean` | No | Filtra empresas con buenas calificaciones. |
| `bad_rating` | `boolean` | No | Filtra empresas con malas calificaciones. |

> Los parámetros aceptan variantes en camelCase, snake_case, kebab-case o CapitalCase.

---

## Ejemplo de Respuesta

```json
{
  "data": [
    {
      "created_by": null,
      "uuid": "00000000-0000-0000-0000-000000000000",
      "name": "Empresa Ejemplo",
      "assumed_name": "Nombre Comercial Ejemplo",
      "slug": "empresa-ejemplo",
      "created_at": "2025-01-01T00:00:00-03:00",
      "address": "Calle Ejemplo 123, Ciudad Ejemplo, EX",
      "fiscal_documents": [
        {
          "id": 1,
          "type": "ruc",
          "value": "00000001"
        }
      ],
      "logos": [
        {
          "unique_id": "123456",
          "usage": "logo",
          "url": "https://example.com/logo.svg",
          "name": "logo.svg",
          "slug": "logo",
          "width": 250,
          "height": 250,
          "creation_date": "2025-01-01 00:00:00"
        }
      ],
      "contacts": [
        {
          "id": 1,
          "type": "email",
          "email": "contacto@ejemplo.com"
        },
        {
          "id": 2,
          "type": "telephone",
          "phone": "+1 234 567 8900",
          "country_code": "+1",
          "number": "234 567 8900"
        }
      ],
      "complaints_count": 0,
      "reviews_count": 0,
      "complaints_exists": false,
      "reviews_exists": false
    }
  ]
}
```

---

## Estructura JSON

| Campo | Tipo | Descripción |
| ----- | ---- | ----------- |
| `data[]` | `array` | Lista de resúmenes de empresas. |
| `data[].uuid` | `uuid` | Identificador de la empresa. |
| `data[].name` | `string` | Nombre de la empresa. |
| `data[].slug` | `string` | Slug apto para URL. |
| `data[].assumed_name` | `string` | Nombre comercial, si existe. |
| `data[].created_at` | `datetime` | Fecha de creación. |
| `data[].address` | `string` | Dirección de la empresa cuando `relations` incluye `address`. |
| `data[].fiscal_documents[]` | `array` | Documentos fiscales cuando `relations` incluye `fiscalDocuments`. |
| `data[].fiscal_documents[].id` | `integer` | ID del documento fiscal. |
| `data[].fiscal_documents[].type` | `string` | Tipo de documento fiscal. |
| `data[].fiscal_documents[].value` | `string` | Valor del documento fiscal. |
| `data[].logos[]` | `array` | Logos de la empresa cuando `relations` incluye `logos`. |
| `data[].contacts[]` | `array` | Contactos de la empresa cuando `relations` incluye `contacts`. |
| `data[].complaints_count` | `integer` | Conteo de reclamaciones cuando `with_count` incluye `complaints`. |
| `data[].reviews_count` | `integer` | Conteo de reseñas cuando `with_count` incluye `reviews`. |
| `data[].complaints_exists` | `boolean` | Indica si hay reclamaciones cuando `with_exists` incluye `complaints`. |
| `data[].reviews_exists` | `boolean` | Indica si hay reseñas cuando `with_exists` incluye `reviews`. |

---

## Notas

* Los metadatos de paginación se omiten cuando `no_paginate` es verdadero.
* Las relaciones, conteos y verificaciones de existencia solo aparecen cuando se solicitan.
* Los filtros de cadena son coincidencias parciales sin distinción entre mayúsculas y minúsculas, salvo indicación contraria.

