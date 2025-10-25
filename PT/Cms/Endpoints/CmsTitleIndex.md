# Cms – Listar Títulos

## Endpoint

```
GET /api/v1/cms/titles
```

## Autenticação

Nenhuma

## Cabeçalhos

| Header           | Type   | Required | Description |
| ---------------- | ------ | -------- | ----------- |
| X-PUBLIC-KEY     | string | Sim      | Chave pública da plataforma. |
| Accept-Language  | string | Não      | Locale IETF (ex., `pt-BR`, `en`, `es`). |

## Parâmetros

### Parâmetros de rota

Nenhum.

### Parâmetros de consulta

| Parâmetro      | Tipo    | Obrigatório | Descrição | Valores |
| -------------- | ------- | ----------- | --------- | ------- |
| language       | string  | Não         | Filtrar por código de idioma. | |
| uuid           | string  | Não         | Filtrar por UUID. | |
| all_language   | boolean | Não         | Incluir todos os idiomas. | `false` |
| is_default     | boolean | Não         | Filtrar por tradução padrão. | |
| usage          | string  | Não         | Filtrar por contexto de uso exato. | |
| usage_like     | string  | Não         | Filtrar por contexto de uso (correspondência parcial). | |
| with_text      | boolean | Não         | Incluir relação de texto. | `false` |
| with_texts     | boolean | Não         | Incluir relação de textos. | `false` |
| per_page       | integer | Não         | Resultados por página. | `25` (1-100) |
| page           | integer | Não         | Número da página. | `1` |
| no_paginate    | boolean | Não         | Desabilitar paginação. | `false` |
| sort           | string  | Não         | Campo de ordenação. | `content` |
| direction      | string  | Não         | Direção da ordenação. | `asc` (`asc`, `desc`) |

> Nomes de parâmetros aceitam variantes `snake_case`, `camelCase`, `kebab-case` ou `CapitalCase`.

## Exemplos

### Exemplo de requisição (curl)

```bash
curl -X GET \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: pt-BR" \
  "https://sandbox.your-domain.com/api/v1/cms/titles?language=pt&usage=homepage.hero&per_page=10&page=1"
```

### Exemplo de resposta

```json
{
  "data": [
    {
      "uuid": "9d4e8f2a-1234-5678-90ab-cdef12345678",
      "language": "pt",
      "is_default": true,
      "content": "Bem-vindo à Nossa Plataforma",
      "usage": "homepage.hero",
      "created_at": "2025-10-23T12:00:00Z",
      "updated_at": "2025-10-23T12:00:00Z",
      "text": {
        "uuid": "8c3d7e1b-5678-90ab-cdef-123456789012",
        "language": "pt",
        "is_default": true,
        "content": "Bem-vindo à Nossa Plataforma"
      }
    }
  ],
  "links": {
    "first": "https://sandbox.your-domain.com/api/v1/cms/titles?page=1",
    "last": "https://sandbox.your-domain.com/api/v1/cms/titles?page=5",
    "prev": null,
    "next": "https://sandbox.your-domain.com/api/v1/cms/titles?page=2"
  },
  "meta": {
    "current_page": 1,
    "from": 1,
    "last_page": 5,
    "path": "https://sandbox.your-domain.com/api/v1/cms/titles",
    "per_page": 10,
    "to": 10,
    "total": 50
  }
}
```

## Estrutura JSON Explicada

| Campo              | Tipo     | Descrição |
| ------------------ | -------- | --------- |
| data               | array    | Coleção de recursos de título. |
| data[].uuid        | string   | Identificador UUID do título. |
| data[].language    | string   | Código de idioma (ISO 639-1). |
| data[].is_default  | boolean  | Se é a tradução padrão. |
| data[].content     | string   | Conteúdo do título. |
| data[].usage       | string   | Identificador de contexto de uso. |
| data[].created_at  | datetime | Timestamp de criação no formato ISO 8601. |
| data[].updated_at  | datetime | Timestamp da última atualização no formato ISO 8601. |
| data[].text        | object   | Recurso de texto relacionado (quando solicitado). |
| data[].text.uuid   | string   | Identificador UUID do texto. |
| data[].text.language | string | Código de idioma do texto. |
| data[].text.is_default | boolean | Se o texto é tradução padrão. |
| data[].text.content | string  | Conteúdo do texto. |
| data[].titles      | array    | Coleção de títulos relacionados (quando solicitado). |
| links              | object   | Links de paginação. |
| meta               | object   | Metadados de paginação. |

## Status HTTP

- 200: OK
- 400: Requisição Inválida
- 401: Não Autorizado
- 403: Proibido
- 404: Não Encontrado
- 422: Entidade Não Processável
- 429: Muitas Requisições
- 500: Erro Interno do Servidor

## Notas

- O parâmetro `language` filtra títulos pelo código de idioma especificado.
- Quando `language` é fornecido, as relações `text` e `titles` serão carregadas para esse idioma ou traduções padrão.
- O parâmetro `usage_like` permite correspondência parcial no campo de uso.
- O parâmetro `no_paginate` retorna todos os resultados sem paginação.
- A ordenação padrão é por `content` em ordem ascendente.
- Todos os timestamps estão no formato ISO 8601.

## Relacionados

- [Criar Título Cms](./CmsTitleStore.md)
- [Mostrar Título Cms](./CmsTitleShow.md)
- [Domínio Cms](../README.md)

## Changelog

- 2025-10-23: Atualização com parâmetros de consulta completos e documentação de resposta.
- 2025-10-16: Documentação inicial.
