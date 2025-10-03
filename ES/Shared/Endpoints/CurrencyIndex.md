# Compartido – Indice de Monedas Disponibles

## Endpoint

```
GET /api/v1/currencies
```

Devuelve las monedas soportadas por el backoffice segun `CurrencyEnum`, incluyendo simbolo (`sign`) y nombre nativo (`native_name`).

---

## Autenticacion

Ninguna.

---

## Encabezados

| Encabezado       | Tipo   | Obligatorio | Descripcion |
| ---------------- | ------ | ----------- | ----------- |
| X-PUBLIC-KEY     | string | No          | Incluya cuando el frontend lo envie por convencion. |
| Accept-Language  | string | No          | Locale IETF para textos traducibles (`pt-BR`, `en`, `es`). |

---

## Parametros

### Parametros de consulta

| Parametro | Tipo   | Obligatorio | Descripcion |
| --------- | ------ | ----------- | ----------- |
| `type`    | string | No          | Filtra el resultado a `fiat` o `crypto`. Comparacion case-insensitive. Valores ausentes o invalidos devuelven todas las monedas. |

> El nombre canonico del parametro es `type` (snake_case). Otras variantes no son aceptadas.

---

## Ejemplos

### Ejemplo de solicitud (curl)

```bash
curl -X GET \
  "https://sandbox.ejemplo.com/api/v1/currencies?type=fiat"
```

### Ejemplo de respuesta

```json
{
  "data": [
    {
      "name": "BRL",
      "value": 1,
      "sign": "R$",
      "native_name": "Real Brasileiro"
    },
    {
      "name": "PYG",
      "value": 4,
      "sign": "₲",
      "native_name": "Guarani"
    }
  ]
}
```

---

## Estructura JSON Explicada

| Campo                | Tipo    | Descripcion |
| -------------------- | ------- | ----------- |
| `data[]`             | array   | Lista de monedas disponibles. |
| `data[].name`        | string  | Nombre del enum (`BRL`, `USD`, `BTC`...). |
| `data[].value`       | integer | Valor numerico del enum. |
| `data[].sign`        | string  | Simbolo monetario o ticker. |
| `data[].native_name` | string  | Nombre nativo de la moneda. |

---

## Status HTTP

- 200: Exito
- 400: Solicitud invalida (p.ej., query malformada)
- 500: Error interno

---

## Notas

- `type=fiat` devuelve solo monedas fiduciarias; `type=crypto` devuelve criptoactivos.
- Valores ausentes o no soportados devuelven la lista completa via `CurrencyEnum::cases()`.
- El orden respeta la declaracion del enum.

---

## Changelog

- 2025-10-03: Documentacion actualizada para reflejar el filtro por tipo y la estructura estandar.
