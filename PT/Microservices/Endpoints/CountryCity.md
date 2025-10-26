# Microservices – CountryCity

## Endpoint

```
GET /api/v1/public/countries/{country}/states/{state}/cities/{city}
```

## Descrição

Retorna os detalhes de uma cidade específica dentro de um estado e país. Valida a hierarquia completa (país > estado > cidade).

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
| country   | string | Yes      | Country identifier (code, name, or slug) |
| state     | string | Yes      | State identifier (name, slug, or code) |
| city      | string | Yes      | City identifier (id, uuid, name, or slug) |

### Query parameters

Nenhum parâmetro de query.

## Exemplos

### Request example (curl)

```bash
# Buscar cidade por nome
curl -X GET \
  -H "X-PUBLIC-KEY: <key>" \
  "https://sandbox.your-domain.com/api/v1/public/countries/BR/states/SP/cities/São%20Paulo"

# Buscar cidade por UUID
curl -X GET \
  -H "X-PUBLIC-KEY: <key>" \
  "https://sandbox.your-domain.com/api/v1/public/countries/BR/states/SP/cities/city-uuid-123"

# Buscar cidade usando slug no estado
curl -X GET \
  -H "X-PUBLIC-KEY: <key>" \
  "https://sandbox.your-domain.com/api/v1/public/countries/BR/states/sao-paulo/cities/campinas"
```

### Response example

```json
{
  "data": {
    "id": 1,
    "uuid": "city-uuid-1",
    "state_id": 1,
    "country_id": 1,
    "name": "São Paulo",
    "abbreviation": "SP",
    "country": {
      "uuid": "550e8400-e29b-41d4-a716-446655440000",
      "name": "Brasil",
      "official_name": "República Federativa do Brasil",
      "code": "BR"
    },
    "state": {
      "uuid": "state-uuid-1",
      "name": "São Paulo",
      "abbreviation": "SP",
      "region": "Sudeste"
    }
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

- Valida toda a hierarquia: país > estado > cidade
- A cidade pode ser identificada por ID, UUID, nome ou slug
- O estado pode ser identificado por nome, slug ou código
- O país é identificado pelo código
- Busca case-insensitive com LIKE para nome e slug
- Sempre inclui os dados do país e estado na resposta quando disponível
- Retorna 404 se a cidade não for encontrada ou não pertencer à hierarquia informada
- Implementado em src/Domain/Microservices/Http/Controllers/CountryController.php:95

## Relacionados

- [Microservices Domínio](../README.md)
- [CountryIndex](./CountryIndex.md)
- [CountryShow](./CountryShow.md)
- [CountryState](./CountryState.md)

## Changelog

- 2025-10-26: Enhanced documentation with detailed parameters and examples
- 2025-10-16: Initial documentation
