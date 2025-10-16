# Company – Contar Empresas por Cobertura

## Endpoint

```
GET /api/v1/company/through-coverage/counter
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
  "https://sandbox.seu-dominio.com/api/v1/company/through-coverage/counter?has_reviews=true"
```

### Exemplo de resposta

```json
{
  "data": {
    "total": 42
  }
}
```

## Estrutura JSON Explicada

| Campo        | Tipo     | Descrição |
| ------------ | -------- | --------- |
| data | object | Contêiner de dados de resposta. |
| data.total | integer | Contagem total de empresas que correspondem aos filtros. |

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

- Este endpoint retorna a contagem total de empresas que correspondem aos filtros fornecidos.
- Todos os parâmetros de filtro do endpoint de índice principal são suportados.
- A contagem respeita as restrições da área de cobertura da plataforma.
- Útil para exibir estatísticas ou informações de paginação sem buscar dados completos.
- Mais eficiente do que buscar todos os resultados quando apenas a contagem é necessária.

## Relacionados

- [Listar Empresas por Cobertura](CompanyThroughCoverageIndex.md)
- [Listar Empresas por Cobertura com Geolocalizações](CompanyThroughCoverageGeolocations.md)
- [Mostrar Empresa](CompanyShow.md)
