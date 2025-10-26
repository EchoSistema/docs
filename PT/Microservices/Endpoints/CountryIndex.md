# Microservices – CountryIndex

## Endpoint

```
GET /api/v1/public/countries
```

## Descrição

Lista países com suporte a filtros avançados por código, capital, moeda, fuso horário e idiomas. Suporta paginação e ordenação customizada.

## Autenticação

Nenhuma

## Cabeçalhos

| Header           | Type   | Required | Description |
| ---------------- | ------ | -------- | ----------- |
| X-PUBLIC-KEY     | string | Yes      | Platform public key. |
| Accept-Language  | string | No       | IETF locale (e.g., `pt-BR`, `en`, `es`). |

## Parâmetros

### Path parameters

No path parameters.

### Query parameters

| Parameter    | Type    | Required | Description | Default/Values |
| ------------ | ------- | -------- | ----------- | -------------- |
| name         | string  | No       | Filter by country name (max 255 chars) | - |
| code         | string  | No       | Filter by country code (max 3 chars) | - |
| cca3         | string  | No       | Filter by CCA3 code (max 3 chars) | - |
| ccn3         | string  | No       | Filter by CCN3 code (max 3 chars) | - |
| cioc         | string  | No       | Filter by CIOC code (max 3 chars) | - |
| capital      | string  | No       | Filter by capital city (max 255 chars) | - |
| currency     | string  | No       | Filter by currency code (max 3 chars) | - |
| timezone     | string  | No       | Filter by timezone (max 255 chars) | - |
| languages    | string  | No       | Filter by language code (max 5 chars) | - |
| minimum      | boolean | No       | Return minimal fields only (id, uuid, code, name, official_name) | false |
| no_paginate  | boolean | No       | Disable pagination and return all results | false |
| per_page     | integer | No       | Results per page | 25 |
| page         | integer | No       | Page number | 1 |
| orderBy      | string  | No       | Field to order results by | name |

## Exemplos

### Request example (curl)

```bash
# Lista todos os países com paginação padrão
curl -X GET \
  -H "X-PUBLIC-KEY: <key>" \
  "https://sandbox.your-domain.com/api/v1/public/countries"

# Filtra por moeda e retorna campos mínimos
curl -X GET \
  -H "X-PUBLIC-KEY: <key>" \
  "https://sandbox.your-domain.com/api/v1/public/countries?currency=USD&minimum=true"

# Retorna todos os países sem paginação
curl -X GET \
  -H "X-PUBLIC-KEY: <key>" \
  "https://sandbox.your-domain.com/api/v1/public/countries?no_paginate=true"
```

### Response example

#### Resposta com campos completos

```json
{
  "data": [
    {
      "id": 1,
      "uuid": "550e8400-e29b-41d4-a716-446655440000",
      "code": "BR",
      "name": "Brasil",
      "official_name": "República Federativa do Brasil",
      "details": {
        "cca3": "BRA",
        "ccn3": "076",
        "currencies": {
          "BRL": {
            "name": "Brazilian real",
            "symbol": "R$"
          }
        },
        "capital": ["Brasília"],
        "timezones": ["UTC-05:00", "UTC-04:00", "UTC-03:00", "UTC-02:00"]
      }
    }
  ],
  "links": {
    "first": "https://sandbox.your-domain.com/api/v1/public/countries?page=1",
    "last": "https://sandbox.your-domain.com/api/v1/public/countries?page=10",
    "prev": null,
    "next": "https://sandbox.your-domain.com/api/v1/public/countries?page=2"
  },
  "meta": {
    "current_page": 1,
    "from": 1,
    "last_page": 10,
    "per_page": 25,
    "to": 25,
    "total": 250
  }
}
```

#### Resposta com campos mínimos (minimum=true)

```json
{
  "data": [
    {
      "id": 1,
      "uuid": "550e8400-e29b-41d4-a716-446655440000",
      "code": "BR",
      "name": "Brasil",
      "official_name": "República Federativa do Brasil"
    }
  ]
}
```

## HTTP Status

- 200: OK
- 201: Created
- 400: Bad Request
- 401: Unauthorized
- 403: Forbidden
- 404: Not Found
- 422: Unprocessable Entity
- 429: Too Many Requests
- 500: Internal Server Error

## Notas

- O parâmetro `minimum=true` retorna apenas os campos essenciais, otimizando a resposta
- Por padrão, a paginação está habilitada com 25 itens por página
- Use `no_paginate=true` para retornar todos os resultados sem paginação
- O campo `details` contém dados brutos do país incluindo moedas, fusos horários, idiomas, etc.
- Os filtros podem ser combinados para refinar os resultados
- O ordenamento padrão é por nome do país
- Todos os nomes de países são traduzidos conforme o header `Accept-Language`

## Relacionados

- [Microservices Domínio](../README.md)
- [CountryShow](./CountryShow.md)
- [CountryState](./CountryState.md)
- [CountryCity](./CountryCity.md)

## Changelog

- 2025-10-26: Enhanced documentation with detailed parameters and examples
- 2025-10-16: Initial documentation
