# Microservices – CountryUpdate

## Endpoint

```
PUT /api/v1/public/countries/{filter}
```

## Descrição

Atualiza os metadados de um país buscando informações atualizadas da API externa RestCountries. Este endpoint sincroniza os dados locais com a fonte oficial.

## Autenticação

Obrigatória – Bearer {token}

## Cabeçalhos

| Header           | Type   | Required | Description |
| ---------------- | ------ | -------- | ----------- |
| Authorization    | string | Yes      | `Bearer {token}`. |
| X-PUBLIC-KEY     | string | Yes      | Platform public key. |
| Accept-Language  | string | No       | IETF locale (e.g., `pt-BR`, `en`, `es`). |

## Parâmetros

### Path parameters

| Parameter | Type   | Required | Description |
| --------- | ------ | -------- | ----------- |
| filter    | string | Yes      | Country identifier (id, code, or name) |

### Query parameters

Nenhum parâmetro de query.

## Exemplos

### Request example (curl)

```bash
# Atualizar país por código
curl -X PUT \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  "https://sandbox.your-domain.com/api/v1/public/countries/BR"

# Atualizar país por nome
curl -X PUT \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  "https://sandbox.your-domain.com/api/v1/public/countries/Brasil"

# Atualizar país por ID
curl -X PUT \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  "https://sandbox.your-domain.com/api/v1/public/countries/1"
```

### Response example

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
      "latlng": [-10.0, -55.0],
      "landlocked": false,
      "area": 8515767.0,
      "flags": {
        "png": "https://flagcdn.com/w320/br.png",
        "svg": "https://flagcdn.com/br.svg"
      },
      "population": 212559417,
      "gini": {
        "2019": 53.4
      },
      "fifa": "BRA",
      "car": {
        "signs": ["BR"],
        "side": "right"
      },
      "continents": ["South America"]
    },
    "states": [
      {
        "id": 1,
        "uuid": "state-uuid-1",
        "abbreviation": "SP",
        "name": "São Paulo",
        "region": "Sudeste"
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

- Requer autenticação via Bearer token
- Busca dados atualizados da API RestCountries (https://restcountries.com)
- Atualiza ou cria o registro do país no banco de dados local (upsert)
- O país pode ser identificado por ID, código (2 letras) ou nome
- Se o código tiver 2 caracteres, busca por `code`, caso contrário por `name`
- Sempre retorna os dados atualizados com estados carregados
- Retorna 404 se o país não for encontrado na API externa
- Implementado em src/Domain/Microservices/Http/Controllers/CountryController.php:67
- Utiliza o gateway RestCountryGateway para buscar dados externos
- Útil para manter os metadados dos países sincronizados

## Relacionados

- [Microservices Domínio](../README.md)
- [CountryIndex](./CountryIndex.md)
- [CountryShow](./CountryShow.md)
- [RestCountryGateway](../../Gateways/RestCountryGateway.md) (se disponível)

## Changelog

- 2025-10-26: Enhanced documentation with detailed parameters and examples
- 2025-10-16: Initial documentation
