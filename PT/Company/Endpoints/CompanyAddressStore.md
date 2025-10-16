# Company – Cadastrar Endereço de Empresa

## Endpoint

```
POST /api/v1/company/{addressable}/addresses
```

## Autenticação

Obrigatória – Bearer {token} com habilidade `store.company.address`

## Cabeçalhos

| Cabeçalho          | Tipo     | Obrigatório | Descrição |
| ------------------ | -------- | ----------- | --------- |
| Authorization      | string   | Sim         | Credencial `Bearer {token}`. |
| X-PUBLIC-KEY       | string   | Sim         | Chave pública da plataforma. |
| Accept-Language    | string   | Não         | Locale IETF (ex.: `pt-BR`, `en`, `es`). |

## Parâmetros

### Parâmetros de caminho

| Parâmetro    | Tipo   | Obrigatório | Descrição |
| ------------ | ------ | ----------- | --------- |
| addressable  | string | Sim         | UUID da empresa. |

### Corpo da requisição

| Parâmetro       | Tipo   | Obrigatório | Descrição | Padrão/Valores |
| --------------- | ------ | ----------- | --------- | -------------- |
| type            | string | Não         | Tipo de endereço: `billing`, `shipping`, ou `both`. | `both` |
| city            | string | Sim         | Nome ou identificador da cidade. | |
| state           | string | Não         | Nome ou identificador do estado. | |
| country_code    | string | Sim         | Código de país ISO 3166-1 alfa-2 (2 caracteres). | |
| address_one     | string | Sim         | Linha de endereço principal. | |
| address_two     | string | Não         | Linha de endereço secundária. | |
| address_three   | string | Não         | Linha de endereço terciária. | |
| address_four    | string | Não         | Linha de endereço quaternária. | |
| zipcode         | string | Não         | Código postal. | |

> O nome canônico dos parâmetros deve ser documentado em `snake_case`. O endpoint aceita variantes (`camelCase`, `kebab-case`, `CapitalCase`).

## Exemplos

### Exemplo de requisição (curl)

```bash
curl -X POST \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <chave>" \
  -H "Accept-Language: pt-BR" \
  -H "Content-Type: application/json" \
  -d '{
    "type": "both",
    "country_code": "PY",
    "state": "Central",
    "city": "Asunción",
    "address_one": "Av. España 123",
    "address_two": "c/ Boquerón",
    "zipcode": "1234"
  }' \
  "https://sandbox.seu-dominio.com/api/v1/company/{company-uuid}/addresses"
```

### Exemplo de resposta

```json
{
  "data": {
    "id": 1,
    "type": "both",
    "country": {
      "id": 1,
      "name": "Paraguay",
      "code": "PY"
    },
    "state": {
      "id": 15,
      "name": "Central"
    },
    "city": {
      "id": 234,
      "name": "Asunción"
    },
    "address_one": "Av. España 123",
    "address_two": "c/ Boquerón",
    "address_three": null,
    "address_four": null,
    "zipcode": "1234",
    "created_at": "2025-10-16T12:00:00-03:00"
  }
}
```

## Estrutura JSON Explicada

| Campo              | Tipo     | Descrição |
| ------------------ | -------- | --------- |
| data               | object   | Recurso de endereço. |
| data.id            | integer  | Identificador do endereço. |
| data.type          | string   | Tipo de endereço: `billing`, `shipping`, ou `both`. |
| data.country       | object   | Informações do país. |
| data.country.id    | integer  | Identificador do país. |
| data.country.name  | string   | Nome do país. |
| data.country.code  | string   | Código de país ISO 3166-1 alfa-2. |
| data.state         | object   | Informações do estado (quando disponível). |
| data.state.id      | integer  | Identificador do estado. |
| data.state.name    | string   | Nome do estado. |
| data.city          | object   | Informações da cidade. |
| data.city.id       | integer  | Identificador da cidade. |
| data.city.name     | string   | Nome da cidade. |
| data.address_one   | string   | Linha de endereço principal. |
| data.address_two   | string   | Linha de endereço secundária. |
| data.address_three | string   | Linha de endereço terciária. |
| data.address_four  | string   | Linha de endereço quaternária. |
| data.zipcode       | string   | Código postal. |
| data.created_at    | datetime | Data e hora de criação. |

## Status HTTP

- 201: Criado
- 400: Requisição inválida
- 401: Não autenticado
- 403: Proibido
- 404: Não encontrado
- 422: Erro de validação
- 429: Limite de requests excedido
- 500: Erro interno

## Erros

```json
{
  "message": "Os dados fornecidos são inválidos.",
  "errors": {
    "city": ["O campo city é obrigatório."],
    "country_code": ["O campo country code é obrigatório."],
    "address_one": ["O campo address one é obrigatório."]
  }
}
```

## Notas

- O parâmetro `type` tem valor padrão `both` se não fornecido.
- O sistema resolve automaticamente as informações da cidade a partir dos dados fornecidos.
- O usuário autenticado deve possuir a habilidade `store.company.address`.
- O parâmetro `addressable` na rota deve ser um UUID válido de empresa.

## Relacionados

- [Company Show](./CompanyShow.md)
- [Company Through Coverage Index](./CompanyThroughCoverageIndex.md)

## Changelog

- 2025-10-16: Documentação inicial.
