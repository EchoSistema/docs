# Cms – Mostrar Título

## Endpoint

```
GET /api/v1/cms/titles/{title}
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

| Parâmetro | Tipo   | Obrigatório | Descrição |
| --------- | ------ | ----------- | --------- |
| title     | string | Sim         | Identificador UUID do título. |

### Parâmetros de consulta

| Parâmetro | Tipo   | Obrigatório | Descrição | Valores |
| --------- | ------ | ----------- | --------- | ------- |
| language  | string | Não         | Filtrar relações por código de idioma. | |

> Nomes de parâmetros aceitam variantes `snake_case`, `camelCase`, `kebab-case` ou `CapitalCase`.

## Exemplos

### Exemplo de requisição (curl)

```bash
curl -X GET \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: pt-BR" \
  "https://sandbox.your-domain.com/api/v1/cms/titles/9d4e8f2a-1234-5678-90ab-cdef12345678?language=pt"
```

### Exemplo de resposta

```json
{
  "data": {
    "uuid": "9d4e8f2a-1234-5678-90ab-cdef12345678",
    "language": "pt",
    "is_default": true,
    "content": "Bem-vindo à Nossa Plataforma",
    "usage": "homepage.hero",
    "raw": {
      "subtitle": "Sua jornada começa aqui",
      "text": "Descubra recursos e serviços incríveis projetados para ajudá-lo a ter sucesso."
    },
    "created_at": "2025-10-23T12:00:00Z",
    "updated_at": "2025-10-23T12:00:00Z",
    "text": {
      "uuid": "8c3d7e1b-5678-90ab-cdef-123456789012",
      "language": "pt",
      "is_default": true,
      "content": "Bem-vindo à Nossa Plataforma"
    },
    "titles": [
      {
        "uuid": "7b2c6d0a-4567-89ab-cdef-0123456789ab",
        "language": "pt",
        "is_default": true,
        "content": "Visão Geral da Plataforma"
      }
    ]
  }
}
```

## Estrutura JSON Explicada

| Campo              | Tipo     | Descrição |
| ------------------ | -------- | --------- |
| data               | object   | Recurso de título. |
| data.uuid          | string   | Identificador UUID do título. |
| data.language      | string   | Código de idioma (ISO 639-1). |
| data.is_default    | boolean  | Se é a tradução padrão. |
| data.content       | string   | Conteúdo do título. |
| data.usage         | string   | Identificador de contexto de uso. |
| data.raw           | object   | Dados adicionais armazenados como JSON. |
| data.raw.subtitle  | string   | Texto do subtítulo. |
| data.raw.text      | string   | Conteúdo de texto estendido. |
| data.created_at    | datetime | Timestamp de criação no formato ISO 8601. |
| data.updated_at    | datetime | Timestamp da última atualização no formato ISO 8601. |
| data.text          | object   | Recurso de texto relacionado. |
| data.text.uuid     | string   | Identificador UUID do texto. |
| data.text.language | string   | Código de idioma do texto. |
| data.text.is_default | boolean | Se o texto é tradução padrão. |
| data.text.content  | string   | Conteúdo do texto. |
| data.titles        | array    | Coleção de títulos relacionados. |
| data.titles[].uuid | string   | Identificador UUID do título relacionado. |
| data.titles[].language | string | Código de idioma do título relacionado. |
| data.titles[].is_default | boolean | Se o título relacionado é tradução padrão. |
| data.titles[].content | string | Conteúdo do título relacionado. |

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

- O parâmetro de rota `title` deve ser um UUID válido.
- Quando `language` é fornecido, as relações `text` e `titles` serão carregadas para esse idioma ou traduções padrão.
- Todos os timestamps estão no formato ISO 8601.

## Relacionados

- [Listar Títulos Cms](./CmsTitleIndex.md)
- [Criar Título Cms](./CmsTitleStore.md)
- [Domínio Cms](../README.md)

## Changelog

- 2025-10-23: Atualização com documentação completa de resposta e parâmetros de consulta.
- 2025-10-16: Documentação inicial.
