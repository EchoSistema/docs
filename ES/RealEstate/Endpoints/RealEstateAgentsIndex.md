# Bienes Raíces — Agentes: Listado

## Endpoint

```
GET /real-estate/agents
```

## Autenticación

No es necesaria.

## Encabezados

| Encabezado      | Tipo   | Obligatorio | Descripción |
| --------------- | ------ | ----------- | ----------- |
| Accept-Language | string | No          | Localiza campos de texto cuando existan traducciones. |

## Parámetros de Consulta

| Parámetro  | Tipo   | Obligatorio | Descripción | Predeterminado |
| ---------- | ------ | ----------- | ----------- | -------------- |
| public_key | string | Sí          | Clave pública de la plataforma (`pbk`). |
| per_page   | int    | No          | Tamaño de página (1–200). | 9 |
| page       | int    | No          | Número de página. | 1 |

## Ejemplo

### Solicitud

```bash
curl -X GET "https://api.ejemplo.com/real-estate/agents?public_key=realestate-demo&per_page=9"
```

### Respuesta (200)

```json
{
  "current_page": 1,
  "data": [
    {
      "uuid": "0f7e3d9a-2a19-4e4b-b0a3-3fb2ef25d001",
      "name": "Laura Nogueira",
      "email": "laura@agency.com",
      "avatar": "https://cdn.ejemplo.com/agents/laura.png",
      "phones": ["+34 600 000 001"],
      "masked_phones": ["+34 •• ••• ••01"],
      "role": "senior-agent",
      "properties": 42
    }
  ],
  "per_page": 9,
  "total": 42
}
```

## Códigos de Estado

- 200 — Correcto
- 400 — Parámetros de paginación inválidos
- 404 — Plataforma no encontrada

## Notas

- Los resultados siempre se filtran por el `public_key` proporcionado.
- La colección se almacena en MongoDB; los campos pueden variar según el cliente.

## Changelog

- 2025-09-25 — Documentación inicial.
