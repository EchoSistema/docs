# Compartido – Indice de Monedas Disponibles

## Endpoint

```
GET /api/v1/currencies
```

Devuelve las monedas soportadas por el backoffice agrupadas en `fiat` y `crypto` segun `CurrencyEnum`.

---

## Autenticacion

Ninguna.

---

## Encabezados

| Encabezado      | Tipo   | Obligatorio | Descripcion |
| ---------------- | ------ | ----------- | ----------- |
| X-PUBLIC-KEY     | string | No          | Incluya cuando el frontend lo envie por convencion. |
| Accept-Language  | string | No          | Locale IETF para textos traducibles (`pt-BR`, `en`, `es`). |

---

## Parametros

### Parametros de consulta

| Parametro | Tipo   | Obligatorio | Descripcion |
| --------- | ------ | ----------- | ----------- |
| `type`    | string | No          | Filtra el resultado a `fiat` o `crypto`. Comparacion case-insensitive. Cuando se informa, el grupo opuesto se devuelve como `null`. Valores ausentes o invalidos devuelven ambos grupos. |

> El nombre canonico del parametro es `type` (snake_case). Otras variantes no son aceptadas.

---

## Ejemplos

### Solicitud (curl)

```bash
curl -X GET \
  "https://sandbox.ejemplo.com/api/v1/currencies?type=crypto"
```

### Respuesta

```json
{
  "data": {
    "crypto": [
      {
        "name": "BTC",
        "value": 7,
        "sign": "₿",
        "native_name": "Bitcoin",
        "type": "crypto"
      },
      {
        "name": "ETH",
        "value": 8,
        "sign": "Ξ",
        "native_name": "Ethereum",
        "type": "crypto"
      }
    ],
    "fiat": null
  }
}
```

---

## Estructura JSON Explicada

| Campo                    | Tipo            | Descripcion |
| ------------------------ | --------------- | ----------- |
| `data.crypto`            | array \| null   | Lista de criptoactivos cuando existe. |
| `data.fiat`              | array \| null   | Lista de monedas fiduciarias cuando existe. |
| `data.*[].name`          | string          | Nombre del enum (`BRL`, `USD`, `BTC`...). |
| `data.*[].value`         | integer         | Valor numerico del enum. |
| `data.*[].sign`          | string          | Simbolo monetario o ticker. |
| `data.*[].native_name`   | string          | Nombre nativo de la moneda. |
| `data.*[].type`          | string          | Grupo (`fiat` o `crypto`). |

---

## Status HTTP

- 200: Exito
- 400: Solicitud invalida (p.ej., query malformada)
- 500: Error interno

---

## Notas

- `type=fiat` devuelve solo el arreglo fiduciario con `data.crypto = null`; `type=crypto` hace lo inverso.
- Sin filtro se devuelven ambos grupos via `CurrencyEnum::getFiat()` y `CurrencyEnum::getCrypto()`.
- El orden respeta la declaracion del enum.

---

## Changelog

- 2025-10-03: Documentacion actualizada para reflejar la respuesta agrupada (`crypto`/`fiat`).
