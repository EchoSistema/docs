# Company – Listar Empresas por Cobertura com Geolocalizações

## Endpoint

```
GET /api/v1/company/through-coverage/geolocations
```

## Autenticação

Nenhuma

## Cabeçalhos

| Cabeçalho          | Tipo     | Obrigatório | Descrição |
| ------------------ | -------- | ----------- | --------- |
| X-PUBLIC-KEY       | string   | Sim         | Chave pública da plataforma. |
| Accept-Language    | string   | Não         | Locale IETF (ex.: `pt-BR`, `en`, `es`). |

## Parâmetros

### Parâmetros de consulta

| Parâmetro | Tipo     | Obrigatório | Descrição | Padrão/Valores |
| --------- | -------- | ----------- | --------- | -------------- |
| id | integer | Não | Filtrar por ID da empresa. | - |
| uuids | array | Não | Filtrar por UUIDs de empresa. | - |
| name | string | Não | Filtrar por nome da empresa (correspondência parcial). | máx: 255 |
| slug | string | Não | Filtrar por slug da empresa. | máx: 255 |
| fiscal_document_type | string | Não | Tipo de documento fiscal. Obrigatório com `fiscal_document_value`. | valores enum |
| fiscal_document_value | string | Não | Valor do documento fiscal. | máx: 255 |
| sort_by | string | Não | Campo para ordenação. | name (máx: 255) |
| order_by | string | Não | Direção da ordenação. | ASC (máx: 4) |
| no_complaints | boolean | Não | Filtrar empresas sem reclamações. | false |
| has_complaints | boolean | Não | Filtrar empresas com reclamações. | false |
| no_reviews | boolean | Não | Filtrar empresas sem avaliações. | false |
| has_reviews | boolean | Não | Filtrar empresas com avaliações. | false |
| rating | numeric | Não | Filtrar por avaliação. | 0-5 |
| good_rating | boolean | Não | Filtrar empresas com boa avaliação. | false |
| bad_rating | boolean | Não | Filtrar empresas com má avaliação. | false |

> Os nomes dos parâmetros estão documentados em `snake_case`. O endpoint aceita variantes (`camelCase`, `kebab-case`, `CapitalCase`).

## Exemplos

### Exemplo de requisição (curl)

```bash
curl -X GET \
  -H "X-PUBLIC-KEY: <chave>" \
  -H "Accept-Language: pt-BR" \
  "https://sandbox.seu-dominio.com/api/v1/company/through-coverage/geolocations?name=Exemplo"
```

### Exemplo de resposta

```json
{
  "data": [
    {
      "uuid": "123e4567-e89b-12d3-a456-426614174000",
      "name": "Empresa Exemplo",
      "slug": "empresa-exemplo",
      "geolocation": {
        "id": 1,
        "latitude": "-25.4284",
        "longitude": "-49.2733"
      }
    }
  ]
}
```

## Estrutura JSON Explicada

| Campo        | Tipo     | Descrição |
| ------------ | -------- | --------- |
| data[] | array | Lista de empresas com geolocalizações. |
| data[].uuid | string | UUID da empresa. |
| data[].name | string | Nome da empresa. |
| data[].slug | string | Identificador amigável da empresa. |
| data[].geolocation | object | Dados de geolocalização (null se não disponível). |
| data[].geolocation.id | integer | ID de geolocalização. |
| data[].geolocation.latitude | string | Coordenada de latitude. |
| data[].geolocation.longitude | string | Coordenada de longitude. |

## Status HTTP

- 200: Sucesso
- 400: Requisição inválida
- 401: Não autenticado
- 422: Erro de validação
- 429: Limite de requests excedido
- 500: Erro interno

## Erros

Erros de validação padrão:

```json
{
  "message": "Os dados fornecidos são inválidos.",
  "errors": {
    "rating": ["A avaliação deve estar entre 0 e 5."]
  }
}
```

## Notas

- Este endpoint retorna apenas empresas que possuem dados de geolocalização no endereço.
- Empresas sem dados de geolocalização são automaticamente excluídas.
- A resposta não é paginada e retorna todos os resultados correspondentes.
- Útil para exibir empresas em mapas ou recursos baseados em localização.
- Todos os parâmetros de filtro do endpoint de índice principal são suportados.

## Relacionados

- [Listar Empresas por Cobertura](CompanyThroughCoverageIndex.md)
- [Contar Empresas por Cobertura](CompanyThroughCoverageCounter.md)
- [Mostrar Empresa](CompanyShow.md)
