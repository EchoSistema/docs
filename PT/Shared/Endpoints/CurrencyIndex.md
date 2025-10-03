# Compartilhado – Listagem de Moedas Disponiveis

## Endpoint

```
GET /api/v1/currencies
```

Retorna as moedas suportadas pelo backoffice, incluindo o simbolo (`sign`) e o nome nativo (`native_name`) definidos em `CurrencyEnum`.

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
| `type`    | string | Nao         | Filtra o resultado para `fiat` ou `crypto`. Comparacao case-insensitive. Valores ausentes ou invalidos retornam todas as moedas. |

> O nome canonico do parametro e `type` (snake_case). Variantes nao sao aceitas.

---

## Exemplos

### Exemplo de requisicao (curl)

```bash
curl -X GET \
  "https://sandbox.exemplo.com/api/v1/currencies?type=fiat"
```

### Exemplo de resposta

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
      "name": "USD",
      "value": 2,
      "sign": "$",
      "native_name": "US Dollar"
    }
  ]
}
```

---

## Estrutura JSON Explicada

| Campo                | Tipo    | Descricao |
| -------------------- | ------- | --------- |
| `data[]`             | array   | Lista de moedas disponiveis. |
| `data[].name`        | string  | Nome do enum (`BRL`, `USD`, `BTC`...). |
| `data[].value`       | integer | Valor numerico associado ao enum. |
| `data[].sign`        | string  | Simbolo monetario ou ticker. |
| `data[].native_name` | string  | Nome nativo da moeda. |

---

## Status HTTP

- 200: Sucesso
- 400: Requisicao invalida (ex.: query malformada)
- 500: Erro interno

---

## Notas

- `type=fiat` retorna apenas moedas fiduciarias; `type=crypto` retorna somente criptoativos.
- Valores diferentes de `fiat`/`crypto` ou ausentes resultam na lista completa conforme `CurrencyEnum::cases()`.
- A ordenacao segue a declaracao do enum.

---

## Changelog

- 2025-10-03: Documentacao revisada para refletir o filtro por tipo e estrutura padronizada.
