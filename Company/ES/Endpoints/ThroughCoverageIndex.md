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
| `no_paginate` | `boolean` | No | Deshabilita la paginación cuando es `true`. |
| `per_page` | `integer` | No | Ítems por página cuando paginado. |
| `sort_by` | `string` | No | Columna para ordenar. |
| `order_by` | `string` | No | Dirección de la ordenación (`ASC` o `DESC`). |
| `with_count` | `array` | No | Relaciones para agregación de conteo. |
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
      "uuid": "5ca829a0-aab2-3991-a8de-817d33b7dbcf",
      "name": "1 Empresa XPTO",
      "assumed_name": null,
      "slug": "1-empresa-xpto",
      "created_at": "2025-06-30T05:11:58-03:00",
      "address": "General Cabañas, 233, Juan León Mallorquín, Encarnación, Itapuá, PY",
      "fiscal_documents": [
        {
          "id": 1002,
          "type": "ruc",
          "value": "123561"
        }
      ]
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
| `data[].address` | `string` | Dirección de la empresa. |
| `data[].fiscal_documents[]` | `array` | Lista de documentos fiscales. |
| `data[].fiscal_documents[].id` | `integer` | ID del documento fiscal. |
| `data[].fiscal_documents[].type` | `string` | Tipo de documento fiscal. |
| `data[].fiscal_documents[].value` | `string` | Valor del documento fiscal. |

---

## Notas

* Los metadatos de paginación se omiten cuando `no_paginate=true`.
* Los filtros de cadena son coincidencias parciales sin distinción entre mayúsculas y minúsculas, salvo indicación contraria.

