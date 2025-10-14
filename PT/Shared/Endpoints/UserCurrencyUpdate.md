# Shared – Atualizar moeda do usuário

## Endpoint

```
PATCH /api/v1/user/currency
```

Atualiza a moeda preferida do usuário autenticado, garantindo que o valor pertença à enum `CurrencyEnum`.

## Autenticação

Obrigatória – Bearer {token} (Laravel Sanctum).

## Cabeçalhos

| Cabeçalho       | Tipo   | Obrigatório | Descrição |
| --------------- | ------ | ----------- | --------- |
| Authorization   | string | Sim         | Credencial `Bearer {token}` válida para o usuário atual. |
| X-PUBLIC-KEY    | string | Sim         | Chave pública da plataforma. |
| Accept-Language | string | Não         | Locale IETF (ex.: `pt-BR`, `en`, `es`). |

## Corpo da requisição

```json
{
  "currency": "BRL"
}
```

| Campo    | Tipo   | Obrigatório | Descrição |
| -------- | ------ | ----------- | --------- |
| currency | string | Sim         | Nome da enum `CurrencyEnum` (ex.: `BRL`, `USD`, `EUR`, `PYG`, `USDC`, `USDT`, `BTC`, `ETH`, `DOGE`, `XMR`, `SOL`). |

## Exemplos

### Requisição (curl)

```bash
curl -X PATCH \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <chave>" \
  -H "Accept-Language: pt-BR" \
  -H "Content-Type: application/json" \
  -d '{"currency":"BRL"}' \
  "https://sandbox.echosistema.com/api/v1/user/currency"
```

### Resposta de sucesso

```json
{
  "data": {
    "currency": "BRL"
  },
  "meta": []
}
```

## Estrutura JSON Explicada

| Campo           | Tipo   | Descrição |
| --------------- | ------ | --------- |
| data.currency   | string | Moeda persistida para o usuário. |
| meta            | array  | Reservado para metadados (vazio). |

## Status HTTP

- 200: Moeda atualizada com sucesso.
- 401: Token ausente ou inválido.
- 422: Valor de `currency` fora dos permitidos.
- 500: Erro interno inesperado.

## Erros

```json
{
  "message": "A moeda selecionada é inválida.",
  "errors": {
    "currency": [
      "O campo currency é inválido."
    ]
  }
}
```

## Notas

- Nome da rota: `api.v1.user.currency.update`.
- A moeda é persistida em `regional_information.currency_id` utilizando a enum `CurrencyEnum`.
- Caso o usuário não possua registro de regionalização, ele é criado automaticamente.

## Relacionados

- [Shared – Atualizar idioma do usuário](UserLanguageUpdate.md)
- [Shared – Listagem de moedas disponíveis](CurrencyIndex.md)

## Changelog

- 2025-10-14: Endpoint documentado.
