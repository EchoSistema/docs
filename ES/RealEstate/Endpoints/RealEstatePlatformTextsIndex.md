# Bienes Raíces — Textos de la Plataforma: Listado

## Endpoint

```
GET /real-estate/platform-texts
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
curl -X GET "https://api.ejemplo.com/real-estate/platform-texts?public_key=realestate-demo"
```

### Respuesta (200)

```json
{
  "data": [
    {
      "key": "hero.title",
      "text": "Encuentra tu próximo hogar",
      "pbk": "realestate-demo"
    },
    {
      "key": "hero.subtitle",
      "text": "Propiedades curadas con agentes verificados",
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

- Los registros provienen de la colección MongoDB `platform_texts` y pueden almacenar variaciones por idioma.
- Se devuelven todos los textos asociados al `public_key` en una sola respuesta.

## Changelog

- 2025-09-25 — Documentación inicial.
