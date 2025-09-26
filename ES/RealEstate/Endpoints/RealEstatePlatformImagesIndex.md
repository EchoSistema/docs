# Bienes Raíces — Imágenes de la Plataforma: Listado

## Endpoint

```
GET /real-estate/platform-images
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
curl -X GET "https://api.ejemplo.com/real-estate/platform-images?public_key=realestate-demo"
```

### Respuesta (200)

```json
{
  "data": [
    {
      "base": "card",
      "data": {
        "url": "https://cdn.ejemplo.com/real-estate/card-default.png",
        "alt": "Imagen principal"
      },
      "pbk": "realestate-demo"
    }
  ],
  "meta": {
    "total": 1
  }
}
```

## Códigos de Estado

- 200 — Correcto
- 404 — Plataforma no encontrada

## Notas

- Los registros son objetos flexibles guardados en la colección MongoDB `platform_images`.
- Se devuelven todos los elementos asociados al `public_key` en una única respuesta.

## Changelog

- 2025-09-25 — Documentación inicial.
