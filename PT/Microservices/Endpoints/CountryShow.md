# Microservices – CountryShow

## Endpoint

```
GET /api/v1/public/countries/{country}
```

## Descrição

Retorna os detalhes completos de um país específico. O país pode ser identificado por ID, UUID, código ou nome.

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

### Query parameters

| Parameter | Type    | Required | Description | Default/Values |
| --------- | ------- | -------- | ----------- | -------------- |
| minimum   | boolean | No       | Return minimal fields only | false |
| states    | string/array | No | Filter states by id or name | - |

## Exemplos

### Request example (curl)

```bash
# Buscar país por código
curl -X GET \
  -H "X-PUBLIC-KEY: <key>" \
  "https://sandbox.your-domain.com/api/v1/public/countries/BR"

# Buscar país por UUID
curl -X GET \
  -H "X-PUBLIC-KEY: <key>" \
  "https://sandbox.your-domain.com/api/v1/public/countries/550e8400-e29b-41d4-a716-446655440000"

# Buscar país com estados filtrados
curl -X GET \
  -H "X-PUBLIC-KEY: <key>" \
  "https://sandbox.your-domain.com/api/v1/public/countries/BR?states=SP,RJ"
```

### Response example

#### Resposta completa

```json
{
  "data": {
    "id": 1,
    "uuid": "550e8400-e29b-41d4-a716-446655440000",
    "code": "BR",
    "name": "Brasil",
    "official_name": "República Federativa do Brasil",
    "details": {
      "cca2": "BR",
      "cca3": "BRA",
      "ccn3": "076",
      "cioc": "BRA",
      "currencies": {
        "BRL": {
          "name": "Brazilian real",
          "symbol": "R$"
        }
      },
      "capital": ["Brasília"],
      "altSpellings": ["BR", "Brasil", "Federative Republic of Brazil"],
      "region": "Americas",
      "subregion": "South America",
      "languages": {
        "por": "Portuguese"
      },
      "timezones": [
        "UTC-05:00",
        "UTC-04:00",
        "UTC-03:00",
        "UTC-02:00"
      ],
      "flags": {
        "png": "https://flagcdn.com/w320/br.png",
        "svg": "https://flagcdn.com/br.svg"
      }
    },
    "states": [
      {
        "id": 1,
        "uuid": "state-uuid-1",
        "abbreviation": "SP",
        "name": "São Paulo",
        "region": "Sudeste"
      },
      {
        "id": 2,
        "uuid": "state-uuid-2",
        "abbreviation": "RJ",
        "name": "Rio de Janeiro",
        "region": "Sudeste"
      }
    ]
  }
}
```

#### Resposta mínima (minimum=true)

```json
{
  "data": {
    "id": 1,
    "uuid": "550e8400-e29b-41d4-a716-446655440000",
    "code": "BR",
    "name": "Brasil",
    "official_name": "República Federativa do Brasil"
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

- O país pode ser identificado por ID, UUID, código (cca2) ou nome
- O parâmetro `states` aceita IDs ou nomes de estados para filtrar a lista
- O campo `details` contém todos os dados brutos do país (moedas, bandeiras, idiomas, etc.)
- Os estados são carregados via relationship eager loading quando disponível
- Nome do país é traduzido conforme o header `Accept-Language`
- Retorna 404 se o país não for encontrado

## Relacionados

- [Microservices Domínio](../README.md)
- [CountryIndex](./CountryIndex.md)
- [CountryState](./CountryState.md)
- [CountryCity](./CountryCity.md)

## Changelog

- 2025-10-26: Enhanced documentation with detailed parameters and examples
- 2025-10-16: Initial documentation
