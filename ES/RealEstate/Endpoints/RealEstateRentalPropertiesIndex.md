# Bienes Raíces — Propiedades en Alquiler: Listado

## Endpoint

```
GET /real-estate/rental-properties
```

## Autenticación

No requiere token; basta con el contexto de plataforma.

## Encabezados

| Encabezado      | Tipo   | Obligatorio | Descripción |
| --------------- | ------ | ----------- | ----------- |
| Accept-Language | string | No          | Idioma preferido para títulos y descripciones. |

## Parámetros de Consulta

### Paginación y Alternancia

| Parámetro   | Tipo    | Obligatorio | Descripción | Predeterminado |
| ----------- | ------- | ----------- | ----------- | -------------- |
| per_page    | int     | No          | Elementos por página cuando hay paginación. | 15 |
| page        | int     | No          | Número de página. | 1 |
| no_paginate | boolean | No          | Si es `true`, devuelve toda la colección sin paginar. | false |

### Localización y Filtros Generales

| Parámetro        | Tipo   | Descripción |
| ---------------- | ------ | ----------- |
| language         | string | Idioma para resolver traducciones (`es`, `pt-BR`, `en`, ...). |
| payment_interval | string | Intervalo de pago (`daily`, `weekly`, `monthly`, ...). |
| is_available     | boolean| Indicador de disponibilidad (el endpoint lo fuerza a `true`). |
| uuid             | uuid   | Filtra por UUID de la propiedad. |
| user_uuid        | uuid   | Filtra por el creador. |
| type             | string | Filtra por slug del tipo asociado. |

### Dirección

| Parámetro | Tipo  | Descripción |
| --------- | ----- | ----------- |
| zipcode   | string| Código postal. |
| city / city_id / city_uuid / city_name | mixed | Acepta nombre, ID numérico o UUID de ciudad. |
| state / state_id / state_uuid / state_name | mixed | Equivalente para provincia/estado. |
| country / country_id / country_uuid / country_code / country_name | mixed | Búsqueda por país (ID, UUID o código ISO). |

### Texto

| Parámetro   | Tipo   | Descripción |
| ----------- | ------ | ----------- |
| title       | string | Filtra por título traducido. |
| description | string | Filtra por descripción. |

## Ejemplo

### Solicitud

```bash
curl -X GET \
  "https://api.ejemplo.com/real-estate/rental-properties?language=es&per_page=12"
```

### Respuesta (200)

```json
{
  "data": [
    {
      "uuid": "c51823c1-8d41-477e-a3b7-7108faae0001",
      "payment_interval": "monthly",
      "type": {
        "uuid": "b7174d07-7e28-4a9b-8f9a-36f0f5270101",
        "name": "Apartamento amueblado"
      },
      "title": "Estudio cerca del metro",
      "renter": "Skyline Rentals",
      "renter_type": "company",
      "image": {
        "usage": "card",
        "url": "https://cdn.ejemplo.com/rental-properties/c51823c1/card.jpg",
        "name": "cover"
      },
      "address": "Av. Diagonal 1000 — Barcelona, Cataluña",
      "is_available": true
    }
  ],
  "links": {
    "first": "https://api.ejemplo.com/real-estate/rental-properties?page=1",
    "last": "https://api.ejemplo.com/real-estate/rental-properties?page=8",
    "prev": null,
    "next": "https://api.ejemplo.com/real-estate/rental-properties?page=2"
  },
  "meta": {
    "current_page": 1,
    "from": 1,
    "last_page": 8,
    "path": "https://api.ejemplo.com/real-estate/rental-properties",
    "per_page": 12,
    "to": 12,
    "total": 92
  }
}
```

## Códigos de Estado

- 200 — Correcto
- 400 — Filtros inválidos
- 403 — La propiedad no pertenece a la plataforma activa
- 500 — Error interno

## Notas

- El endpoint obliga `is_available = true`.
- Los nombres de tipo, títulos y descripciones siguen el parámetro `language` con fallback al idioma por defecto.
- Con `no_paginate=true` se omiten `links` y `meta`, pero los elementos continúan dentro de `data`.

## Changelog

- 2025-09-25 — Documentación inicial.
