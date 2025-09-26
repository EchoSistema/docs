# Bienes Raíces — Propiedades en Alquiler: Detalle

## Endpoint

```
GET /real-estate/rental-properties/{rentalProperty}
```

## Autenticación

Solo verifica que la propiedad pertenezca a la plataforma activa; no se requiere token Bearer.

## Parámetros de Ruta

| Parámetro      | Tipo | Obligatorio | Descripción |
| -------------- | ---- | ----------- | ----------- |
| rentalProperty | uuid | Sí          | UUID o identificador aceptado por el binding de `RentableItem`. |

## Parámetros de Consulta

| Parámetro | Tipo   | Obligatorio | Descripción |
| --------- | ------ | ----------- | ----------- |
| language  | string | No          | Idioma para títulos y descripciones (`es`, `pt-BR`, `en`, ...). |

## Ejemplo

### Solicitud

```bash
curl -X GET "https://api.ejemplo.com/real-estate/rental-properties/c51823c1-8d41-477e-a3b7-7108faae0001?language=es"
```

### Respuesta (200)

```json
{
  "data": {
    "uuid": "c51823c1-8d41-477e-a3b7-7108faae0001",
    "payment_interval": "monthly",
    "type": {
      "uuid": "b7174d07-7e28-4a9b-8f9a-36f0f5270101",
      "name": "Apartamento amueblado"
    },
    "creator": {
      "uuid": "49287c4a-32aa-483c-8d4f-98cfd8300101",
      "name": "Administrador de alquiler"
    },
    "title": "Estudio cerca del metro",
    "description": "Estudio totalmente amueblado, servicios incluidos.",
    "renter": "Skyline Rentals",
    "renter_type": "company",
    "image": {
      "usage": "card",
      "url": "https://cdn.ejemplo.com/rental-properties/c51823c1/card.jpg",
      "name": "cover"
    },
    "images": [
      {
        "usage": "gallery",
        "url": "https://cdn.ejemplo.com/rental-properties/c51823c1/gallery-1.jpg",
        "name": "sala",
        "width": 1920,
        "height": 1080
      }
    ],
    "address": {
      "street": "Av. Diagonal 1000",
      "city": "Barcelona",
      "state": "Cataluña",
      "country": "ES",
      "zipcode": "08019",
      "formatted": "Av. Diagonal 1000 — Barcelona, Cataluña"
    },
    "is_available": true
  }
}
```

## Códigos de Estado

- 200 — Correcto
- 403 — La propiedad no pertenece a la plataforma activa
- 404 — Propiedad no encontrada

## Notas

- El controlador valida que el arrendador corresponda a la compañía de la plataforma (`App::platform()->company`).
- Los campos traducidos siguen el parámetro `language` y, en su defecto, utilizan el idioma predeterminado.
- La respuesta incluye la imagen de tarjeta y la galería completa disponible.

## Changelog

- 2025-09-25 — Documentación inicial.
