# ReputationBook – Listar estados de reclamación

## Endpoint

```
GET /api/v1/complaints/statuses
```

Devuelve todos los valores de `ComplaintStatusEnum`, incluyendo el valor técnico, el nombre de la enumeración y el título localizado.

## Autenticación

Ninguna. Solo se exige el encabezado `X-PUBLIC-KEY` por el middleware `platform`.

## Encabezados

| Encabezado | Tipo | Obligatorio | Descripción |
| ---------- | ---- | ----------- | ----------- |
| `X-PUBLIC-KEY` | string | Sí | Identifica la plataforma solicitante. |
| `Accept-Language` | string | Opcional | Ayuda al frontend a elegir el idioma; el endpoint ya devuelve `title()` traducido. |

## Ejemplo de respuesta (200)

```json
{
  "data": [
    {
      "value": "open",
      "name": "OPEN",
      "title": "Abierta"
    },
    {
      "value": "in_progress",
      "name": "IN_PROGRESS",
      "title": "En progreso"
    },
    {
      "value": "resolved",
      "name": "RESOLVED",
      "title": "Resuelta"
    }
  ]
}
```

## Estructura JSON

| Campo | Tipo | Descripción |
| ------ | ---- | ----------- |
| `data[]` | array | Lista de todos los estados disponibles. |
| `data[].value` | string | Valor persistido en la enum. |
| `data[].name` | string | Nombre de la constante en mayúsculas. |
| `data[].title` | string | Etiqueta localizada para presentación. |

## Códigos HTTP

- 200: Lista devuelta.
- 500: Error inesperado.

