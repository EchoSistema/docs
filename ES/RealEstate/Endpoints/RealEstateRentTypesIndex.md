# Bienes Raíces — Tipos de Alquiler: Listado

## Endpoint

```
GET /real-estate/rent-types
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
curl -X GET "https://api.ejemplo.com/real-estate/rent-types?public_key=realestate-demo"
```

### Respuesta (200)

```json
{
  "data": [
    {
      "title": "Diario",
      "value": "daily",
      "pbk": "realestate-demo"
    },
    {
      "title": "Mensual",
      "value": "monthly",
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

- Información obtenida desde la colección MongoDB `rent_types`.
- El listado devuelve todos los registros sin paginación.

## Changelog

- 2025-09-25 — Documentación inicial.
