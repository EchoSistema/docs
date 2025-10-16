# Company – Listar Oportunidades de Emprego

## Endpoint

```
GET /api/v1/company/opportunities
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

| Parâmetro   | Tipo     | Obrigatório | Descrição | Padrão/Valores |
| ----------- | -------- | ----------- | --------- | -------------- |
| language    | string   | Não         | Código de idioma para campos traduzíveis (ex.: `en`, `pt-BR`, `es`). | Padrão da plataforma |
| noPaginate  | boolean  | Não         | Desabilitar paginação quando `true`. | `false` |
| perPage     | integer  | Não         | Número de itens por página quando paginado. | 25 |
| page        | integer  | Não         | Número da página quando paginado. | 1 |
| withTrash   | boolean  | Não         | Incluir oportunidades excluídas. | `false` |

> O nome canônico dos parâmetros deve ser documentado em `snake_case`. O endpoint aceita variantes (`camelCase`, `kebab-case`, `CapitalCase`).

## Exemplos

### Exemplo de requisição (curl)

```bash
curl -X GET \
  -H "X-PUBLIC-KEY: <chave>" \
  -H "Accept-Language: pt-BR" \
  "https://sandbox.seu-dominio.com/api/v1/company/opportunities?language=pt-BR&perPage=10"
```

### Exemplo de resposta (paginada)

```json
{
  "data": [
    {
      "uuid": "00000000-0000-0000-0000-000000000001",
      "company": "Tech Solutions Inc.",
      "total_job_openings": 3,
      "title": "Desenvolvedor de Software Sênior",
      "mode": "remote",
      "starts_at": "2025-10-20T00:00:00+00:00",
      "finishes_at": "2025-11-30T23:59:59+00:00"
    },
    {
      "uuid": "00000000-0000-0000-0000-000000000002",
      "company": "Tech Solutions Inc.",
      "total_job_openings": 2,
      "title": "Engenheiro DevOps",
      "mode": "hybrid",
      "starts_at": "2025-10-15T00:00:00+00:00",
      "finishes_at": "2025-12-15T23:59:59+00:00"
    }
  ],
  "links": {
    "first": "https://sandbox.seu-dominio.com/api/v1/company/opportunities?page=1",
    "last": "https://sandbox.seu-dominio.com/api/v1/company/opportunities?page=5",
    "prev": null,
    "next": "https://sandbox.seu-dominio.com/api/v1/company/opportunities?page=2"
  },
  "meta": {
    "current_page": 1,
    "from": 1,
    "last_page": 5,
    "path": "https://sandbox.seu-dominio.com/api/v1/company/opportunities",
    "per_page": 10,
    "to": 10,
    "total": 47
  }
}
```

## Estrutura JSON Explicada

| Campo                    | Tipo     | Descrição |
| ------------------------ | -------- | --------- |
| data[]                   | array    | Lista de oportunidades de emprego. |
| data[].uuid              | string   | UUID da oportunidade de emprego. |
| data[].company           | string   | Nome da empresa (apenas quando não é proprietário). |
| data[].total_job_openings| integer  | Número de vagas abertas. |
| data[].title             | string   | Título do trabalho no idioma solicitado. |
| data[].mode              | string   | Modo de trabalho: `remote`, `hybrid`, `on-site`. |
| data[].starts_at         | datetime | Data de início da oportunidade. |
| data[].finishes_at       | datetime | Data de término da oportunidade. |
| links                    | object   | Links de paginação (quando paginado). |
| links.first              | string   | URL para a primeira página. |
| links.last               | string   | URL para a última página. |
| links.prev               | string   | URL para a página anterior. |
| links.next               | string   | URL para a próxima página. |
| meta                     | object   | Metadados de paginação (quando paginado). |
| meta.current_page        | integer  | Número da página atual. |
| meta.from                | integer  | Índice do item inicial. |
| meta.last_page           | integer  | Número total de páginas. |
| meta.path                | string   | Caminho base da URL. |
| meta.per_page            | integer  | Itens por página. |
| meta.to                  | integer  | Índice do item final. |
| meta.total               | integer  | Número total de itens. |

## Status HTTP

- 200: Sucesso
- 400: Requisição inválida
- 401: Não autenticado
- 404: Não encontrado
- 429: Limite de requests excedido
- 500: Erro interno

## Erros

```json
{
  "message": "Chave de plataforma inválida.",
  "errors": {}
}
```

## Notas

- Retorna oportunidades de emprego para a empresa associada à plataforma atual (identificada por `X-PUBLIC-KEY`).
- As oportunidades são ordenadas por `finishes_at` em ordem decrescente (mais recente primeiro).
- Quando `noPaginate=true`, os metadados de paginação (`links` e `meta`) são omitidos.
- O campo `company` é incluído na resposta apenas quando a requisição não é do proprietário.
- Oportunidades excluídas são excluídas por padrão. Use `withTrash=true` para incluí-las.
- Títulos e descrições de trabalhos são retornados no idioma solicitado, retornando ao idioma padrão se não estiver disponível.

## Relacionados

- [Job Opportunity Show](./MeJobOpportunityShow.md)
- [Company Show](./CompanyShow.md)

## Changelog

- 2025-10-16: Documentação inicial.
