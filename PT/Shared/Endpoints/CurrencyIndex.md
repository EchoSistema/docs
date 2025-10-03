# Compartilhado – Listagem de Moedas Disponíveis

## Endpoint

`GET /api/v1/currencies`

Retorna as moedas suportadas pelo backoffice, incluindo símbolo (`sign`) e nome nativo (`native_name`). Permite filtrar por tipo (`fiat` ou `crypto`).

---

## Autenticação

Nenhuma.

---

## Requisição

### Parâmetros de query

| Parâmetro | Tipo | Obrigatório | Descrição |
| --------- | ---- | ----------- | --------- |
| `type` | `string` | Não | Filtra o resultado para `fiat` ou `crypto`. A comparação é case-insensitive. Quando ausente ou inválido, retorna todas as moedas. |

> O parâmetro deve ser enviado como `type` (snake_case). Variantes com outras grafias não são reconhecidas.

---

## Exemplo de Resposta

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

## Estrutura JSON Explicada

| Campo | Tipo | Descrição |
| ----- | ---- | --------- |
| `data[]` | `array` | Lista de moedas disponíveis. |
| `data[].name` | `string` | Código da moeda (enum). |
| `data[].value` | `integer` | Valor numérico da moeda no enum. |
| `data[].sign` | `string` | Símbolo monetário ou ticker. |
| `data[].native_name` | `string` | Nome nativo da moeda. |

---

## Notas

* `type=fiat` retorna apenas moedas fiduciárias; `type=crypto` retorna somente criptoativos.
* Valores diferentes de `fiat`/`crypto` ou ausentes resultam na lista completa.
* A ordenação segue a ordem definida no enum `CurrencyEnum`.

---

## Changelog

- 2025-10-03: Documentação inicial do endpoint e filtro por tipo (`fiat`/`crypto`).
