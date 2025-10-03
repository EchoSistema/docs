# Compartido – Índice de Monedas Disponibles

## Endpoint

`GET /api/v1/currencies`

Devuelve las monedas soportadas por el backoffice, incluyendo símbolo (`sign`) y nombre nativo (`native_name`). Permite filtrar por tipo (`fiat` o `crypto`).

---

## Autenticación

Ninguna.

---

## Request

### Parámetros de consulta

| Parámetro | Tipo | Obligatorio | Descripción |
| --------- | ---- | ----------- | ----------- |
| `type` | `string` | No | Filtra el resultado a `fiat` o `crypto`. La comparación es case-insensitive. Cuando está ausente o es inválido, se devuelven todas las monedas. |

> El parámetro debe enviarse como `type` (snake_case). Otras variantes no son reconocidas.

---

## Ejemplo de Respuesta

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
      "name": "BTC",
      "value": 7,
      "sign": "₿",
      "native_name": "Bitcoin"
    }
  ]
}
```

---

## Estructura JSON Explicada

| Campo | Tipo | Descripción |
| ----- | ---- | ----------- |
| `data[]` | `array` | Lista de monedas disponibles. |
| `data[].name` | `string` | Código de la moneda (nombre del enum). |
| `data[].value` | `integer` | Valor numérico del enum. |
| `data[].sign` | `string` | Símbolo monetario o ticker. |
| `data[].native_name` | `string` | Nombre nativo de la moneda. |

---

## Notas

* `type=fiat` devuelve solo monedas fiduciarias; `type=crypto` devuelve solo criptoactivos.
* Valores ausentes o inválidos devuelven la lista completa.
* El orden sigue la declaración del enum `CurrencyEnum`.

---

## Changelog

- 2025-10-03: Documentación inicial del endpoint con filtro por tipo (`fiat`/`crypto`).
