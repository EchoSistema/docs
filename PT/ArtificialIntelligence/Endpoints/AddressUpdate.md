# Inteligência Artificial – Atualizar Endereço

## Endpoint

```
PUT /api/v1/ai/admin/addresses/{address}/type/{type}
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

## Parâmetros da URL

| Parâmetro | Tipo   | Obrigatório | Descrição |
| --------- | ------ | ----------- | --------- |
| address   | string | Sim         | UUID do endereço. |
| type      | string | Sim         | Tipo de endereço para filtrar. Valores válidos: `BILLING`, `SHIPPING`, `FISCAL`, `BOTH` |

## Parâmetros

### Parâmetros do corpo

| Parâmetro    | Tipo   | Obrigatório | Descrição | Padrão/Valores |
| ------------ | ------ | ----------- | --------- | -------------- |
| type         | string | Não         | Tipo de endereço. | Valores válidos: `BILLING`, `SHIPPING`, `FISCAL`, `BOTH` |
| city         | string/object | Não | Nome da cidade ou objeto. | - |
| city_id      | integer | Não        | ID da cidade. | - |
| state_id     | integer | Não        | ID do estado. | - |
| country_id   | integer | Não        | ID do país. | - |
| zipcode      | string | Não         | CEP/Código postal. | Máx: 255 caracteres |
| address_one  | string | Não         | Linha de endereço primária. | Máx: 255 caracteres |
| address_two  | string | Não         | Linha de endereço secundária. | Máx: 255 caracteres |
| address_three| string | Não         | Linha de endereço terciária. | Máx: 255 caracteres |
| address_four | string | Não         | Linha de endereço quaternária. | Máx: 255 caracteres |
| uuid         | -      | -           | Campo proibido. Não pode ser atualizado. | - |

> Os nomes dos parâmetros aceitam variantes `snake_case`, `camelCase`, `kebab-case` ou `CapitalCase`.

## Exemplos

### Exemplo de requisição (curl)

```bash
curl -X PUT \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: pt-BR" \
  -H "Content-Type: application/json" \
  -d '{
    "address_one": "Avenida Paulista, 1578",
    "address_two": "Andar 16",
    "zipcode": "01310-200"
  }' \
  "https://sandbox.your-domain.com/api/v1/ai/admin/addresses/00000000-0000-0000-0000-000000000001/type/BOTH"
```

### Exemplo de requisição (JavaScript)

```javascript
const response = await fetch('https://sandbox.your-domain.com/api/v1/ai/admin/addresses/00000000-0000-0000-0000-000000000001/type/BOTH', {
  method: 'PUT',
  headers: {
    'Authorization': 'Bearer <token>',
    'X-PUBLIC-KEY': '<key>',
    'Accept-Language': 'pt-BR',
    'Content-Type': 'application/json',
  },
  body: JSON.stringify({
    address_one: 'Avenida Paulista, 1578',
    address_two: 'Andar 16',
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
    "two": "Andar 16",
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
    "formatted": "Avenida Paulista, 1578, Andar 16, São Paulo, SP 01310-200, Brasil"
  }
}
```

## Estrutura JSON Explicada

| Campo                   | Tipo    | Descrição |
| ----------------------- | ------- | --------- |
| data                    | object  | Endereço atualizado. |
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
- 401: Não Autorizado
- 403: Proibido
- 404: Não Encontrado
- 422: Entidade Não Processável (erros de validação)
- 429: Muitas Requisições
- 500: Erro Interno do Servidor

## Erros

### Erro de Validação

```json
{
  "message": "O campo uuid é proibido.",
  "errors": {
    "uuid": [
      "O campo uuid é proibido."
    ]
  }
}
```

### Não Encontrado

```json
{
  "message": "Não encontrado"
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

- Este endpoint atualiza um endereço existente para a plataforma do usuário autenticado.
- Apenas os campos fornecidos serão atualizados; todos os outros campos permanecem inalterados.
- O campo `uuid` é proibido e não pode ser atualizado.
- O endereço é consultado tanto pelo UUID quanto pelo parâmetro de tipo na URL.
- A resolução da cidade é tratada através de um pipeline que resolve automaticamente informações de cidade, estado e país quando campos relacionados à cidade são atualizados.
- Você pode atualizar a cidade usando uma string/objeto no campo `city` ou fornecendo `city_id`, `state_id` e `country_id`.

## Relacionados

- [Criar Endereço](./AddressStore.md)

## Histórico de Alterações

- 2025-10-23: Documentação inicial.
