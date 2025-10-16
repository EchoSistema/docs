# Company – Listar Avaliações de Empresas

## Endpoint

```
GET /api/v1/company/reviews
GET /api/v1/company/reviews/my
```

## Autenticação

- `/api/v1/company/reviews` - Nenhuma
- `/api/v1/company/reviews/my` - Obrigatória - Bearer {token} com habilidade `backoffice`

## Cabeçalhos

| Cabeçalho          | Tipo     | Obrigatório | Descrição |
| ------------------ | -------- | ----------- | --------- |
| Authorization      | string   | Para rota `/my` | Credencial `Bearer {token}`. |
| X-PUBLIC-KEY       | string   | Sim         | Chave pública da plataforma. |
| Accept-Language    | string   | Não         | Locale IETF (ex.: `pt-BR`, `en`, `es`). |

## Parâmetros

### Parâmetros de consulta

| Parâmetro | Tipo     | Obrigatório | Descrição | Padrão/Valores |
| --------- | -------- | ----------- | --------- | -------------- |
| is_public | boolean | Não | Filtrar apenas avaliações públicas. Definido automaticamente como true se não autenticado. | - |
| company | string | Não | Filtrar por UUID ou slug da empresa. | - |
| user | string | Não | Filtrar por UUID do usuário. | - |
| keywords | string | Não | Buscar no texto da avaliação. | - |
| rating | integer | Não | Filtrar por valor da avaliação. | 1-5 |
| created_at | date | Não | Filtrar por data de criação. | data ISO 8601 |
| interval | string | Não | Filtro de intervalo de tempo. | hour, day, week, month, year |
| period | object | Não | Filtro de intervalo de datas. | - |
| period.from | date | Não | Data de início do período. | data ISO 8601 |
| period.to | date | Não | Data de fim do período. | data ISO 8601 |
| with_images | boolean | Não | Filtrar apenas avaliações com imagens. | - |
| sort_by | string | Não | Campo para ordenação. | created_at |
| order_by | string | Não | Direção da ordenação. | desc (ASC, DESC) |
| page | integer | Não | Número da página para paginação. | 1 |
| per_page | integer | Não | Itens por página. | 25 |
| no_paginate | boolean | Não | Desabilitar paginação. | false |
| limit | integer | Não | Limitar resultados quando `no_paginate` é true. | - |

> Os nomes dos parâmetros estão documentados em `snake_case`. O endpoint aceita variantes (`camelCase`, `kebab-case`, `CapitalCase`).

## Exemplos

### Exemplo de requisição (curl)

```bash
curl -X GET \
  -H "X-PUBLIC-KEY: <chave>" \
  -H "Accept-Language: pt-BR" \
  "https://sandbox.seu-dominio.com/api/v1/company/reviews?rating=5&per_page=10"
```

### Exemplo de resposta

```json
{
  "data": [
    {
      "id": 1,
      "uuid": "123e4567-e89b-12d3-a456-426614174000",
      "review": "Excelente serviço e produtos de qualidade.",
      "rating": 5,
      "is_public": true,
      "created_at": "2025-01-15T10:30:00.000000Z",
      "company": {
        "id": 1,
        "uuid": "223e4567-e89b-12d3-a456-426614174000",
        "name": "Empresa Exemplo",
        "slug": "empresa-exemplo",
        "rating": {
          "average": 4.5,
          "total": 120
        },
        "logos": [
          {
            "url": "https://example.com/logo.png",
            "usage": "logo"
          }
        ]
      }
    }
  ],
  "links": {
    "first": "https://sandbox.seu-dominio.com/api/v1/company/reviews?page=1",
    "last": "https://sandbox.seu-dominio.com/api/v1/company/reviews?page=10",
    "prev": null,
    "next": "https://sandbox.seu-dominio.com/api/v1/company/reviews?page=2"
  },
  "meta": {
    "current_page": 1,
    "from": 1,
    "last_page": 10,
    "per_page": 10,
    "to": 10,
    "total": 100
  }
}
```

## Estrutura JSON Explicada

| Campo        | Tipo     | Descrição |
| ------------ | -------- | --------- |
| data[] | array | Lista de avaliações. |
| data[].id | integer | ID da avaliação. |
| data[].uuid | string | UUID da avaliação. |
| data[].review | string | Conteúdo de texto da avaliação. |
| data[].rating | integer | Valor da avaliação (1-5). |
| data[].is_public | boolean | Se a avaliação é pública. |
| data[].created_at | string | Data de criação da avaliação (ISO 8601). |
| data[].company | object | Informações da empresa. |
| data[].company.id | integer | ID da empresa. |
| data[].company.uuid | string | UUID da empresa. |
| data[].company.name | string | Nome da empresa. |
| data[].company.slug | string | Slug da empresa. |
| data[].company.rating | object | Informações de avaliação da empresa. |
| data[].company.rating.average | float | Avaliação média. |
| data[].company.rating.total | integer | Número total de avaliações. |
| data[].company.logos[] | array | Logos da empresa. |
| links | object | Links de paginação. |
| meta | object | Metadados de paginação. |

## Status HTTP

- 200: Sucesso
- 400: Requisição inválida
- 401: Não autenticado (para rota `/my` sem autenticação)
- 422: Erro de validação
- 429: Limite de requests excedido
- 500: Erro interno

## Erros

Erros de validação padrão:

```json
{
  "message": "Os dados fornecidos são inválidos.",
  "errors": {
    "rating": ["A avaliação deve estar entre 1 e 5."]
  }
}
```

## Notas

- Requisições não autenticadas filtram automaticamente apenas avaliações públicas.
- A rota `/my` retorna apenas avaliações do usuário autenticado.
- Usuários com papel de convidado só podem ver suas próprias avaliações.
- A paginação padrão é de 25 itens por página.
- As avaliações incluem dados da empresa relacionados com logos e informações de avaliação.
- Use o parâmetro `keywords` para busca de texto completo no conteúdo das avaliações.

## Relacionados

- [Criar Avaliação de Empresa](CompanyReviewStore.md)
- [Mostrar Empresa](CompanyShow.md)
- [Listar Empresas por Cobertura](CompanyThroughCoverageIndex.md)
