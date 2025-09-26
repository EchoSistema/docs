# Bienes Raíces — Tipos de Propiedad: Listado

## Endpoint

```
GET /real-estate/property-types
```

## Autenticación

No es necesaria.

## Parámetros de Consulta

| Parámetro  | Tipo   | Obligatorio | Descripción |
| ---------- | ------ | ----------- | ----------- |
| public_key | string | Sí          | Clave pública de la plataforma (`pbk`). |

## Ejemplo

### Solicitud

```bash
curl -X GET "https://api.ejemplo.com/real-estate/property-types?public_key=realestate-demo"
```

### Respuesta (200)

```json
{
  "data": [
    {
      "uuid": "7cc944ef-3b77-41a1-9f36-3f0a3c1f9001",
      "slug": "apartment",
      "title": "Apartamento",
      "pbk": "realestate-demo"
    },
    {
      "uuid": "e1840e5b-485d-4ea5-8ea6-64cef0f30102",
      "slug": "house",
      "title": "Casa",
      "pbk": "realestate-demo"
    }
  ],
  "meta": {
    "total": 2
  }
}
```

## Códigos de Estado

- 200 — Correcto
- 404 — Plataforma no encontrada

## Notas

- La colección reside en MongoDB (`property_types`).
- El endpoint devuelve todos los registros sin paginar.

## Changelog

- 2025-09-25 — Documentación inicial.
