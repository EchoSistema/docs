# Inteligência Artificial – Criar Endereço

## Endpoint

```
POST /api/v1/ai/admin/addresses
```

## Autenticação

Obrigatório – Bearer {token} com habilidade `auth:sanctum`

## Cabeçalhos

| Cabeçalho        | Tipo   | Obrigatório | Descrição |
| ---------------- | ------ | ----------- | --------- |
| Authorization    | string | Sim         | `Bearer {token}`. |
| X-PUBLIC-KEY     | string | Sim         | Chave pública da plataforma. |
| Accept-Language  | string | Não         | Locale IETF (ex.: `pt-BR`, `en`, `es`). |
| Content-Type     | string | Sim         | `application/json`. |

## Parâmetros

### Parâmetros do corpo

| Parâmetro    | Tipo   | Obrigatório | Descrição | Padrão/Valores |
| ------------ | ------ | ----------- | --------- | -------------- |
| type         | string | Não         | Tipo de endereço. | Padrão: `BOTH`. Valores válidos: `BILLING`, `SHIPPING`, `FISCAL`, `BOTH` |
| uuid         | string | Não         | UUID personalizado para o endereço. | Gerado automaticamente se não fornecido |
| city         | string/object | Sim | Nome da cidade ou objeto. | - |
| state        | string/object | Não | Nome do estado ou objeto. | - |
| country_code | string | Sim         | Código do país (ISO 3166-1 alpha-2). | Máx: 2 caracteres |
| address_one  | string | Sim         | Linha de endereço primária. | - |
| address_two  | string | Não         | Linha de endereço secundária. | - |
| address_three| string | Não         | Linha de endereço terciária. | - |
| address_four | string | Não         | Linha de endereço quaternária. | - |
| zipcode      | string | Não         | CEP/Código postal. | - |

> Os nomes dos parâmetros aceitam variantes `snake_case`, `camelCase`, `kebab-case` ou `CapitalCase`.

## Exemplos

### Exemplo de requisição (curl)

```bash
curl -X POST \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: pt-BR" \
  -H "Content-Type: application/json" \
  -d '{
    "type": "BOTH",
    "city": "São Paulo",
    "country_code": "BR",
    "address_one": "Avenida Paulista, 1578",
    "address_two": "Andar 15",
    "zipcode": "01310-200"
  }' \
  "https://sandbox.your-domain.com/api/v1/ai/admin/addresses"
```

### Exemplo de requisição (JavaScript)

```javascript
const response = await fetch('https://sandbox.your-domain.com/api/v1/ai/admin/addresses', {
  method: 'POST',
  headers: {
    'Authorization': 'Bearer <token>',
    'X-PUBLIC-KEY': '<key>',
    'Accept-Language': 'pt-BR',
    'Content-Type': 'application/json',
  },
  body: JSON.stringify({
    type: 'BOTH',
    city: 'São Paulo',
    country_code: 'BR',
    address_one: 'Avenida Paulista, 1578',
    address_two: 'Andar 15',
    zipcode: '01310-200'
  })
});
```

### Exemplo de resposta

```json
{
  "data": {
    "uuid": "00000000-0000-0000-0000-000000000001",
    "main": true,
    "type": "BOTH",
    "zipcode": "01310-200",
    "one": "Avenida Paulista, 1578",
    "two": "Andar 15",
    "three": null,
    "four": null,
    "city": {
      "uuid": "00000000-0000-0000-0000-000000000002",
      "name": "São Paulo",
      "abbreviation": "SP"
    },
    "state": {
      "uuid": "00000000-0000-0000-0000-000000000003",
      "name": "São Paulo",
      "abbreviation": "SP",
      "region": "Sudeste"
    },
    "country": {
      "uuid": "00000000-0000-0000-0000-000000000004",
      "name": "Brasil",
      "official_name": "República Federativa do Brasil",
      "code": "BR"
    },
    "formatted": "Avenida Paulista, 1578, Andar 15, São Paulo, SP 01310-200, Brasil"
  }
}
```

## Estrutura JSON Explicada

| Campo                   | Tipo    | Descrição |
| ----------------------- | ------- | --------- |
| data                    | object  | Endereço criado. |
| data.uuid               | string  | UUID do endereço. |
| data.main               | boolean | Se este é o endereço principal (verdadeiro quando o tipo é `BOTH`). |
| data.is_billing         | boolean | Se este é um endereço de cobrança (presente quando o tipo não é `BOTH`). |
| data.is_shipping        | boolean | Se este é um endereço de entrega (presente quando o tipo não é `BOTH`). |
| data.is_fiscal          | boolean | Se este é um endereço fiscal (presente quando o tipo não é `BOTH`). |
| data.type               | string  | Tipo de endereço. |
| data.zipcode            | string  | CEP/Código postal. |
| data.one                | string  | Linha de endereço primária. |
| data.two                | string  | Linha de endereço secundária. |
| data.three              | string  | Linha de endereço terciária. |
| data.four               | string  | Linha de endereço quaternária. |
| data.city               | object  | Informações da cidade. |
| data.city.uuid          | string  | UUID da cidade. |
| data.city.name          | string  | Nome da cidade. |
| data.city.abbreviation  | string  | Abreviação da cidade. |
| data.state              | object  | Informações do estado. |
| data.state.uuid         | string  | UUID do estado. |
| data.state.name         | string  | Nome do estado. |
| data.state.abbreviation | string  | Abreviação do estado. |
| data.state.region       | string  | Região do estado. |
| data.country            | object  | Informações do país. |
| data.country.uuid       | string  | UUID do país. |
| data.country.name       | string  | Nome do país. |
| data.country.official_name | string | Nome oficial do país. |
| data.country.code       | string  | Código do país (ISO 3166-1 alpha-2). |
| data.formatted          | string  | Endereço formatado. |

## Status HTTP

- 200: OK
- 201: Criado
- 401: Não Autorizado
- 403: Proibido
- 422: Entidade Não Processável (erros de validação)
- 429: Muitas Requisições
- 500: Erro Interno do Servidor

## Erros

### Erro de Validação

```json
{
  "message": "O campo city é obrigatório.",
  "errors": {
    "city": [
      "O campo city é obrigatório."
    ]
  }
}
```

### Não Autenticado

```json
{
  "message": "Não autenticado.",
  "errors": {}
}
```

## Notas

- Este endpoint cria um novo endereço para a plataforma do usuário autenticado.
- O campo `type` tem como padrão `BOTH` se não for fornecido.
- A resolução da cidade é tratada através de um pipeline que resolve automaticamente informações de cidade, estado e país.
- Se `uuid` não for fornecido, será gerado automaticamente.
- O `country_code` deve ser um código válido ISO 3166-1 alpha-2 (ex.: "US", "BR", "ES").

## Relacionados

- [Atualizar Endereço](./AddressUpdate.md)

## Histórico de Alterações

- 2025-10-23: Documentação inicial.
