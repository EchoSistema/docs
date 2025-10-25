# Cms – Criar Título

## Endpoint

```
POST /api/v1/cms/titles
```

## Autenticação

Obrigatória – Bearer {token} com habilidade `store.platform.title`

## Cabeçalhos

| Header           | Type   | Required | Description |
| ---------------- | ------ | -------- | ----------- |
| Authorization    | string | Sim      | `Bearer {token}`. |
| X-PUBLIC-KEY     | string | Sim      | Chave pública da plataforma. |
| Accept-Language  | string | Não      | Locale IETF (ex., `pt-BR`, `en`, `es`). |

## Parâmetros

### Parâmetros de rota

Nenhum.

### Corpo da requisição

| Parâmetro       | Tipo    | Obrigatório | Descrição | Valores |
| --------------- | ------- | ----------- | --------- | ------- |
| language        | string  | Sim         | Código de idioma (ISO 639-1). | `en`, `es`, `pt`, `gn` |
| is_default      | boolean | Sim         | Se é a tradução padrão. | |
| content         | string  | Sim         | Conteúdo do título (máx. 255 caracteres). | |
| title           | string  | Não         | Alias para o campo `content`. | |
| usage           | string  | Não         | Identificador de contexto de uso (máx. 255 caracteres). | |
| additional_data | object  | Não         | Objeto de dados adicionais. | |
| additional_data.subtitle | string | Não | Texto do subtítulo (máx. 255 caracteres). | |
| additional_data.text     | string | Não | Conteúdo de texto estendido (máx. 5000 caracteres). | |

> Nomes de parâmetros aceitam variantes `snake_case`, `camelCase`, `kebab-case` ou `CapitalCase`.
>
> **Nota:** O campo `additional_data` será convertido para `raw` internamente pelo manipulador de requisição.

## Exemplos

### Exemplo de requisição (curl)

```bash
curl -X POST \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: pt-BR" \
  -H "Content-Type: application/json" \
  -d '{
    "language": "pt",
    "is_default": true,
    "content": "Bem-vindo à Nossa Plataforma",
    "usage": "homepage.hero",
    "additional_data": {
      "subtitle": "Sua jornada começa aqui",
      "text": "Descubra recursos e serviços incríveis projetados para ajudá-lo a ter sucesso."
    }
  }' \
  "https://sandbox.your-domain.com/api/v1/cms/titles"
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
    }
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

## Status HTTP

- 201: Criado
- 400: Requisição Inválida
- 401: Não Autorizado
- 403: Proibido
- 404: Não Encontrado
- 422: Entidade Não Processável
- 429: Muitas Requisições
- 500: Erro Interno do Servidor

## Erros

```json
{
  "message": "Os dados fornecidos são inválidos.",
  "errors": {
    "language": ["O campo language é obrigatório."],
    "is_default": ["O campo is default é obrigatório."],
    "content": ["O campo content é obrigatório."]
  }
}
```

## Notas

- O usuário autenticado deve ter a habilidade `store.platform.title`.
- Pode-se usar o campo `content` ou `title`; são aliases.
- O objeto `additional_data` é convertido para o campo `raw` internamente.
- Comprimento máximo para `content` é 255 caracteres.
- Comprimento máximo para `usage` é 255 caracteres.
- Comprimento máximo para `additional_data.subtitle` é 255 caracteres.
- Comprimento máximo para `additional_data.text` é 5000 caracteres.
- Todos os timestamps estão no formato ISO 8601.

## Relacionados

- [Índice de Títulos Cms](./CmsTitleIndex.md)
- [Mostrar Título Cms](./CmsTitleShow.md)
- [Domínio Cms](../README.md)

## Changelog

- 2025-10-23: Atualização com documentação completa de requisição/resposta.
- 2025-10-16: Documentação inicial.
