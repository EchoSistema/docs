# Compartilhado – Listagem de Moedas Disponiveis

## Endpoint

```
GET /api/v1/currencies
```

Retorna as moedas suportadas pelo backoffice, agrupadas em `fiat` e `crypto` conforme o enum `CurrencyEnum`.

---

## Autenticacao

Nenhuma.

---

## Cabecalhos

| Cabecalho       | Tipo   | Obrigatorio | Descricao |
| --------------- | ------ | ----------- | --------- |
| X-PUBLIC-KEY    | string | Nao         | Utilize se o front ja enviar por padrao. |
| Accept-Language | string | Nao         | Locale IETF para textos traduziveis (`pt-BR`, `en`, `es`). |

---

## Parametros

### Parametros de consulta

| Parametro | Tipo   | Obrigatorio | Descricao |
| --------- | ------ | ----------- | --------- |
| `type`    | string | Nao         | Filtra o resultado para `fiat` ou `crypto`. Comparacao case-insensitive. Quando informado, o grupo oposto retorna `null`. Valores ausentes ou invalidos retornam ambos os grupos. |

> O nome canonico do parametro e `type` (snake_case). Variantes nao sao aceitas.

---

## Exemplos

### Exemplo de requisicao (curl)

```bash
curl -X GET \
  "https://sandbox.exemplo.com/api/v1/currencies?type=crypto"
```

### Exemplo de resposta

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

## Estrutura JSON Explicada

| Campo                       | Tipo            | Descricao |
| --------------------------- | --------------- | --------- |
| `data.crypto`               | array \| null   | Lista de criptoativos quando disponivel. |
| `data.fiat`                 | array \| null   | Lista de moedas fiduciarias quando disponivel. |
| `data.*[].name`             | string          | Nome do enum (`BRL`, `USD`, `BTC`...). |
| `data.*[].value`            | integer         | Valor numerico associado ao enum. |
| `data.*[].sign`             | string          | Simbolo monetario ou ticker. |
| `data.*[].native_name`      | string          | Nome nativo da moeda. |
| `data.*[].type`             | string          | Categoria (`fiat` ou `crypto`). |

---

## Status HTTP

- 200: Sucesso
- 400: Requisicao invalida (ex.: query malformada)
- 500: Erro interno

---

## Notas

- `type=fiat` retorna `data.fiat` preenchido e `data.crypto = null`; `type=crypto` faz o oposto.
- Sem filtro, ambos os grupos sao retornados conforme `CurrencyEnum::getFiat()` e `CurrencyEnum::getCrypto()`.
- A ordenacao segue a declaracao do enum.

---

## Changelog

- 2025-10-03: Documentacao revisada para refletir retorno agrupado (`crypto`/`fiat`).
