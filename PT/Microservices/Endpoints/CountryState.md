# Microservices – CountryState

## Endpoint

```
GET /api/v1/public/countries/{country}/states/{state}
```

## Descrição

Retorna os detalhes de um estado específico de um país. Valida se o estado pertence ao país informado.

## Autenticação

Nenhuma

## Cabeçalhos

| Header           | Type   | Required | Description |
| ---------------- | ------ | -------- | ----------- |
| X-PUBLIC-KEY     | string | Yes      | Platform public key. |
| Accept-Language  | string | No       | IETF locale (e.g., `pt-BR`, `en`, `es`). |

## Parâmetros

### Path parameters

| Parameter | Type   | Required | Description |
| --------- | ------ | -------- | ----------- |
| country   | string | Yes      | Country identifier (id, uuid, code, or name) |
| state     | string | Yes      | State identifier (id, uuid, abbreviation, or name) |

### Query parameters

| Parameter | Type         | Required | Description | Default/Values |
| --------- | ------------ | -------- | ----------- | -------------- |
| cities    | string/array | No       | Filter cities by id or name | - |

## Exemplos

### Request example (curl)

```bash
# Buscar estado por abreviação
curl -X GET \
  -H "X-PUBLIC-KEY: <key>" \
  "https://sandbox.your-domain.com/api/v1/public/countries/BR/states/SP"

# Buscar estado por nome
curl -X GET \
  -H "X-PUBLIC-KEY: <key>" \
  "https://sandbox.your-domain.com/api/v1/public/countries/BR/states/São%20Paulo"

# Buscar estado com cidades filtradas
curl -X GET \
  -H "X-PUBLIC-KEY: <key>" \
  "https://sandbox.your-domain.com/api/v1/public/countries/BR/states/SP?cities=São%20Paulo,Campinas"
```

### Response example

#### Resposta completa

```json
{
  "data": {
    "id": 1,
    "uuid": "state-uuid-1",
    "country_id": 1,
    "name": "São Paulo",
    "abbreviation": "SP",
    "region": "Sudeste",
    "country": {
      "uuid": "550e8400-e29b-41d4-a716-446655440000",
      "name": "Brasil",
      "official_name": "República Federativa do Brasil",
      "code": "BR"
    },
    "cities": [
      {
        "id": 1,
        "uuid": "city-uuid-1",
        "name": "São Paulo"
      },
      {
        "id": 2,
        "uuid": "city-uuid-2",
        "name": "Campinas"
      },
      {
        "id": 3,
        "uuid": "city-uuid-3",
        "name": "Santos"
      }
    ]
  }
}
```

#### Resposta com cidades filtradas

```json
{
  "data": {
    "id": 1,
    "uuid": "state-uuid-1",
    "country_id": 1,
    "name": "São Paulo",
    "abbreviation": "SP",
    "region": "Sudeste",
    "country": {
      "uuid": "550e8400-e29b-41d4-a716-446655440000",
      "name": "Brasil",
      "official_name": "República Federativa do Brasil",
      "code": "BR"
    },
    "cities": [
      {
        "id": 1,
        "uuid": "city-uuid-1",
        "name": "São Paulo"
      },
      {
        "id": 2,
        "uuid": "city-uuid-2",
        "name": "Campinas"
      }
    ]
  }
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

- Valida se o estado pertence ao país informado, retornando 404 caso não pertença
- O estado pode ser identificado por ID, UUID, abreviação ou nome
- O país pode ser identificado por ID, UUID, código ou nome
- O parâmetro `cities` aceita IDs ou nomes para filtrar a lista de cidades
- As cidades são carregadas via relationship eager loading quando disponível
- O país é sempre incluído na resposta quando disponível
- Retorna 404 se o estado não for encontrado ou não pertencer ao país

## Relacionados

- [Microservices Domínio](../README.md)
- [CountryIndex](./CountryIndex.md)
- [CountryShow](./CountryShow.md)
- [CountryCity](./CountryCity.md)

## Changelog

- 2025-10-26: Enhanced documentation with detailed parameters and examples
- 2025-10-16: Initial documentation
