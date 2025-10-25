# Shared – Listar Categorias

## Endpoint

```
GET /api/v1/categories
```

## Autenticação

Nenhuma

## Cabeçalhos

| Cabeçalho     | Tipo | Obrigatório | Descrição |
| ---------------- | ------ | -------- | ----------- |
| Authorization    | string | Não | Credencial `Bearer {token}`. |
| X-PUBLIC-KEY     | string | Sim      | Chave pública da plataforma. |
| Accept-Language  | string | Não       | Locale IETF (ex.: `pt-BR`, `en`, `es`). Usado para filtrar os títulos. |

## Parâmetros de Query

| Parâmetro | Tipo | Obrigatório | Descrição |
| ----------- | ------- | ----------- | ----------- |
| domain_area_uuid | string (UUID) | Não | UUID da área de domínio para filtrar categorias. Se não fornecido, usa a área de domínio da plataforma atual. |
| language | string | Não | Código do idioma para filtrar títulos (ex.: `pt-BR`, `en`, `es`). Se não fornecido, retorna títulos padrão. |
| noPaginate | boolean | Não | Se presente, retorna todos os resultados sem paginação. |

## Exemplos

### Exemplo de requisição (curl)

```bash
curl -X GET \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: pt-BR" \
  "https://sandbox.your-domain.com/api/v1/categories?language=pt-BR"
```

### Exemplo de requisição com área de domínio específica

```bash
curl -X GET \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: pt-BR" \
  "https://sandbox.your-domain.com/api/v1/categories?domain_area_uuid=123e4567-e89b-12d3-a456-426614174000&language=pt-BR"
```

### Exemplo de requisição sem paginação

```bash
curl -X GET \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: pt-BR" \
  "https://sandbox.your-domain.com/api/v1/categories?noPaginate=true&language=pt-BR"
```

### Exemplo de resposta (paginada)

```json
{
  "data": [
    {
      "uuid": "123e4567-e89b-12d3-a456-426614174000",
      "domain_area": {
        "uuid": "123e4567-e89b-12d3-a456-426614174001",
        "name": "Ecommerce"
      },
      "group": {
        "uuid": "123e4567-e89b-12d3-a456-426614174002",
        "domain_area": {
          "uuid": "123e4567-e89b-12d3-a456-426614174001",
          "name": "Ecommerce"
        },
        "title": "Eletrônicos",
        "title_language": "pt-BR",
        "is_default": true,
        "slug": "eletronicos"
      },
      "title": "Smartphones",
      "title_is_default": true,
      "title_language": "pt-BR",
      "slug": "smartphones"
    }
  ],
  "links": {
    "first": "https://sandbox.your-domain.com/api/v1/categories?page=1",
    "last": "https://sandbox.your-domain.com/api/v1/categories?page=3",
    "prev": null,
    "next": "https://sandbox.your-domain.com/api/v1/categories?page=2"
  },
  "meta": {
    "current_page": 1,
    "from": 1,
    "last_page": 3,
    "path": "https://sandbox.your-domain.com/api/v1/categories",
    "per_page": 10,
    "to": 10,
    "total": 30
  }
}
```

### Exemplo de resposta (sem paginação)

```json
{
  "data": [
    {
      "uuid": "123e4567-e89b-12d3-a456-426614174000",
      "domain_area": {
        "uuid": "123e4567-e89b-12d3-a456-426614174001",
        "name": "Ecommerce"
      },
      "group": {
        "uuid": "123e4567-e89b-12d3-a456-426614174002",
        "domain_area": {
          "uuid": "123e4567-e89b-12d3-a456-426614174001",
          "name": "Ecommerce"
        },
        "title": "Eletrônicos",
        "title_language": "pt-BR",
        "is_default": true,
        "slug": "eletronicos"
      },
      "title": "Smartphones",
      "title_is_default": true,
      "title_language": "pt-BR",
      "slug": "smartphones"
    }
  ]
}
```

## Estrutura JSON Explicada

### Resposta Paginada

| Campo | Tipo | Descrição |
| ----------- | ------- | ----------- |
| data        | array   | Array de objetos de categoria |
| links       | object  | Links de navegação da paginação |
| links.first | string  | URL da primeira página |
| links.last  | string  | URL da última página |
| links.prev  | string\|null | URL da página anterior (null se primeira página) |
| links.next  | string\|null | URL da próxima página (null se última página) |
| meta        | object  | Metadados da paginação |
| meta.current_page | integer | Página atual |
| meta.from   | integer | Índice do primeiro item na página |
| meta.last_page | integer | Número da última página |
| meta.path   | string  | URL base da rota |
| meta.per_page | integer | Itens por página |
| meta.to     | integer | Índice do último item na página |
| meta.total  | integer | Total de itens |

### Objeto de Categoria (data[])

| Campo | Tipo | Descrição |
| ----------- | ------- | ----------- |
| uuid        | string  | UUID único da categoria |
| domain_area | object\|null | Área de domínio da categoria (apenas se carregada) |
| domain_area.uuid | string | UUID da área de domínio |
| domain_area.name | string | Nome da área de domínio |
| group       | object\|null | Grupo ao qual a categoria pertence (apenas se carregada) |
| group.uuid  | string  | UUID do grupo |
| group.domain_area | object | Área de domínio do grupo |
| group.title | string  | Título do grupo no idioma solicitado ou padrão |
| group.title_language | string | Código do idioma do título |
| group.is_default | boolean | Se o título é o padrão |
| group.slug  | string  | Slug do título do grupo |
| title       | string  | Título da categoria no idioma solicitado ou padrão |
| title_is_default | boolean | Se o título retornado é o padrão |
| title_language | string | Código do idioma do título (ex.: `pt-BR`, `en`, `es`) |
| slug        | string  | Slug gerado a partir do título |

## Status HTTP

- 200: OK
- 201: Criado
- 400: Requisição inválida
- 401: Não autenticado
- 403: Proibido
- 404: Não encontrado
- 422: Erro de validação
- 429: Limite de requests excedido
- 500: Erro interno

## Erros

```json
{
  "message": "Mensagem de erro"
}
```

## Notas

- Listar todas as categorias de uma área de domínio

## Relacionados

- See other Shared API endpoints

## Changelog

- 2025-10-16: Documentação inicial
