# Bienes Raíces — Propiedades: Listado

Expone el catálogo público de propiedades almacenado en MongoDB y aplica los
mismos criterios del `PropertyFilter` (búsqueda por locale, moneda, metadata de
agencia/agente y rangos numéricos). Si `per_page` no se informa, la paginación
devuelve 9 registros por defecto.

## Endpoint

```
GET /real-estate/properties
```

## Autenticación

No es necesaria. Utiliza `public_key` para limitar los resultados a una plataforma concreta.

## Encabezados

| Encabezado      | Tipo   | Obligatorio | Descripción |
| --------------- | ------ | ----------- | ----------- |
| Accept-Language | string | No          | Locale para localizar los campos de texto (`es`, `pt-BR`, `en`, …). |

## Parámetros de Consulta

### Paginación y Ordenación

| Parámetro | Tipo   | Obligatorio | Descripción | Valor por defecto |
| --------- | ------ | ----------- | ----------- | ---------------- |
| page      | int    | No          | Número de página. | 1 |
| per_page  | int    | No          | Tamaño de página (1–200). | 9 |
| sort_by   | string | No          | Campo para ordenar (`created_at`, `size`, `rooms`, `bed`, `bath`, `garage`, `is_featured`, `direct_from_owner`, `value`). | `createdAt` |
| direction | string | No          | `asc` o `desc` (sin distinguir mayúsculas). | `DESC` |

### Localización

| Parámetro | Tipo   | Obligatorio | Descripción | Predeterminado |
| --------- | ------ | ----------- | ----------- | -------------- |
| language  | string | No          | Locale usado al aplicar `search` sobre campos multi-idioma (`en`, `es`, `pt-BR`, …). | — |

### Identificadores y Propiedad

| Parámetro    | Tipo   | Obligatorio | Descripción |
| ------------ | ------ | ----------- | ----------- |
| public_key[] | string | No          | Filtra por clave(s) pública(s) (`pbk`). |
| uuid[]       | uuid   | No          | Filtra por UUID(s) de la propiedad. |
| agency[]     | string | No          | Filtra por nombre de la inmobiliaria (p. ej. `Delta Offices`). |
| agency_uuid[]| uuid   | No          | Filtra por UUID(s) de la inmobiliaria. |
| agent[]      | string | No          | Filtra por nombre del agente. |
| agent_uuid[] | uuid   | No          | Filtra por UUID(s) del agente. |

### Ubicación y Tipología

| Parámetro       | Tipo   | Obligatorio | Descripción |
| --------------- | ------ | ----------- | ----------- |
| city[]          | string | No          | Ciudad. |
| region[]        | string | No          | Región/estado. |
| country[]       | string | No          | Código de país ISO. |
| rent_type[]     | string | No          | Tipo de renta/venta. |
| property_type[] | string | No          | Tipo de propiedad (insensible a mayúsculas/minúsculas). |

### Capacidad y Comodidades

| Parámetro | Tipo | Obligatorio | Descripción |
| --------- | ---- | ----------- | ----------- |
| rooms / rooms_min / rooms_max | int | No | Número de ambientes. |
| bed / bed_min / bed_max       | int | No | Dormitorios (por defecto `bed_min = 1`). |
| bath / bath_min / bath_max    | int | No | Baños (por defecto `bath_min = 1`). |
| size_min / size_max           | int | No | Rango de superficie (por defecto `size_min = 1`, `size_max = 10000`). |
| garage                        | int | No | Plazas de garaje exactas. |

### Destacados y Precio

| Parámetro         | Tipo    | Obligatorio | Descripción |
| ----------------- | ------- | ----------- | ----------- |
| is_featured       | boolean | No          | Solo propiedades destacadas. |
| direct_from_owner | boolean | No          | Solo propiedades directo con el propietario. |
| search            | string  | No          | Búsqueda libre en metadatos. |
| value_currency    | string  | No          | Código de moneda (`EUR`, `USD`, …). Obligatorio si ordenas por `value`. |
| currency          | string  | No          | Alias de `value_currency`, útil para ordenar por precio sin filtrar. |
| value_min/max     | number  | No          | Rango de precio aplicado solamente cuando se envía junto con `value_currency`. |

### Fechas

| Parámetro               | Tipo | Obligatorio | Descripción |
| ----------------------- | ---- | ----------- | ----------- |
| created_at              | date | No          | Fecha exacta de creación. |
| created_at_period[from] | date | No          | Fecha inicial (aliases: `start`, `0`). |
| created_at_period[to]   | date | No          | Fecha final (aliases: `end`, `1`). |

> Los filtros tipo arreglo aceptan parámetros repetidos (`?uuid[]=...`) o notación con índices (`uuid[0]=...`).

## Ejemplo

### Solicitud

```bash
curl -X GET \
  "https://api.ejemplo.com/real-estate/properties?public_key=realestate-demo&city=Barcelona&per_page=9"
```

### Respuesta (200)

```json
{
  "current_page": 1,
  "data": [
    {
      "uuid": "e2c9c1c6-4cac-4c41-8f8e-6fd79d303001",
      "title": "Ático con vista al mar",
      "city": "Barcelona",
      "region": "Cataluña",
      "country": "ES",
      "value": {
        "amount": 875000,
        "currency": "EUR"
      },
      "rentType": "sale",
      "propertyType": "penthouse",
      "rooms": 7,
      "bed": 4,
      "bath": 3,
      "garage": 2,
      "size": 215,
      "isFeatured": true,
      "directFromOwner": false,
      "createdAt": "2025-08-13T12:45:11Z"
    }
  ],
  "first_page_url": "https://api.ejemplo.com/real-estate/properties?page=1",
  "from": 1,
  "last_page": 12,
  "last_page_url": "https://api.ejemplo.com/real-estate/properties?page=12",
  "links": [],
  "next_page_url": "https://api.ejemplo.com/real-estate/properties?page=2",
  "path": "https://api.ejemplo.com/real-estate/properties",
  "per_page": 9,
  "prev_page_url": null,
  "to": 9,
  "total": 108
}
```

## Códigos de Estado

- 200 — Correcto
- 400 — Error de validación en los filtros
- 404 — Plataforma no encontrada
- 500 — Error interno

## Notas

- El `public_key` restringe los resultados a la plataforma indicada.
- `sort_by` acepta alias en camelCase y snake_case.
- Los filtros expuestos reflejan el comportamiento del `PropertyFilter`
  (locale, moneda, agencia/agente y rangos numéricos).
- La respuesta corresponde al paginador estándar `LengthAwarePaginator` de Laravel.

## Changelog

- 2025-09-25 — Documentación inicial.
- 2025-09-25 — Documentados filtros por defecto, búsqueda con `language` y filtrado por nombre de agencia/agente.
