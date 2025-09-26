# Bienes Raíces — Agentes: Detalle

## Endpoint

```
GET /real-estate/agents/{agent}
```

## Autenticación

No es necesaria.

## Parámetros de Ruta

| Parámetro | Tipo | Obligatorio | Descripción |
| --------- | ---- | ----------- | ----------- |
| agent     | uuid | Sí          | UUID del agente. |

## Ejemplo

### Solicitud

```bash
curl -X GET "https://api.ejemplo.com/real-estate/agents/0f7e3d9a-2a19-4e4b-b0a3-3fb2ef25d001"
```

### Respuesta (200)

```json
{
  "data": {
    "uuid": "0f7e3d9a-2a19-4e4b-b0a3-3fb2ef25d001",
    "name": "Laura Nogueira",
    "email": "laura@agency.com",
    "avatar": "https://cdn.ejemplo.com/agents/laura.png",
    "phones": ["+34 600 000 001"],
    "masked_phones": ["+34 •• ••• ••01"],
    "role": "senior-agent",
    "properties": 42,
    "socials": {
      "instagram": "@laura.moves",
      "linkedin": "laura-nogueira"
    }
  }
}
```

## Códigos de Estado

- 200 — Correcto
- 404 — Agente no encontrado

## Notas

- Los datos provienen de la colección MongoDB `agents`.
- El UUID ya es único por plataforma, por lo que `public_key` es opcional.

## Changelog

- 2025-09-25 — Documentación inicial.
