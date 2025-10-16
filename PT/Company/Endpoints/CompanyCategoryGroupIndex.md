# Company – Listar Grupos de Categorias de Empresa

## Endpoint

```
GET /api/v1/company/category-groups
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

| Parâmetro  | Tipo     | Obrigatório | Descrição | Padrão/Valores |
| ---------- | -------- | ----------- | --------- | -------------- |
| language   | string   | Não         | Código de idioma para títulos (ex.: `en`, `pt-BR`, `es`). | Padrão da plataforma |
| categories | boolean  | Não         | Incluir categorias dentro dos grupos. | `false` |

> O nome canônico dos parâmetros deve ser documentado em `snake_case`. O endpoint aceita variantes (`camelCase`, `kebab-case`, `CapitalCase`).

## Exemplos

### Exemplo de requisição (curl)

```bash
curl -X GET \
  -H "X-PUBLIC-KEY: <chave>" \
  -H "Accept-Language: pt-BR" \
  "https://sandbox.seu-dominio.com/api/v1/company/category-groups?language=pt-BR&categories=true"
```

### Exemplo de resposta

```json
{
  "data": [
    {
      "uuid": "00000000-0000-0000-0000-000000000001",
      "title": "Serviços de Tecnologia",
      "group": {
        "uuid": "00000000-0000-0000-0000-000000000010",
        "category": [
          {
            "uuid": "00000000-0000-0000-0000-000000000100",
            "title": "Desenvolvimento de Software"
          },
          {
            "uuid": "00000000-0000-0000-0000-000000000101",
            "title": "Suporte de TI"
          }
        ]
      },
      "domain_area": {
        "uuid": "00000000-0000-0000-0000-000000000020",
        "name": "Tecnologia"
      }
    }
  ]
}
```

## Estrutura JSON Explicada

| Campo                        | Tipo     | Descrição |
| ---------------------------- | -------- | --------- |
| data[]                       | array    | Lista de grupos de categorias. |
| data[].uuid                  | string   | UUID do grupo de categorias. |
| data[].title                 | string   | Título do grupo no idioma solicitado. |
| data[].group                 | object   | Detalhes do grupo (quando `categories=true`). |
| data[].group.uuid            | string   | UUID do grupo. |
| data[].group.category[]      | array    | Lista de categorias no grupo. |
| data[].group.category[].uuid | string   | UUID da categoria. |
| data[].group.category[].title| string   | Título da categoria no idioma solicitado. |
| data[].domain_area           | object   | Informações da área de domínio. |
| data[].domain_area.uuid      | string   | UUID da área de domínio. |
| data[].domain_area.name      | string   | Nome da área de domínio. |

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

- Retorna apenas grupos de categorias associados à plataforma atual (identificada por `X-PUBLIC-KEY`).
- Quando `language` não é fornecido, o sistema usa o idioma padrão da plataforma.
- Se um título no idioma solicitado não estiver disponível, o sistema retorna para o idioma padrão.
- Quando `categories=false` ou omitido, apenas informações do grupo de categorias são retornadas sem categorias aninhadas.

## Relacionados

- [Company Through Coverage Index](./CompanyThroughCoverageIndex.md)

## Changelog

- 2025-10-16: Documentação inicial.
