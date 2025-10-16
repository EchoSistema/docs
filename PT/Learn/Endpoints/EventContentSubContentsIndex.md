# Learn – Listar Sub-Conteúdos do Evento

## Endpoint

```
GET /api/v1/learn/events/{event}/contents/{index}/sub-contents
```

## Autenticação

Obrigatória – Bearer {token}

## Cabeçalhos

| Cabeçalho       | Tipo   | Obrigatório | Descrição |
| --------------- | ------ | ----------- | --------- |
| Authorization   | string | Sim         | `Bearer {token}`. |
| X-PUBLIC-KEY    | string | Sim         | Chave pública da plataforma. |
| Accept-Language | string | Não         | Locale IETF (ex.: `pt-BR`, `en`, `es`). |

## Parâmetros

### Parâmetros de caminho

| Parâmetro | Tipo    | Obrigatório | Descrição |
| --------- | ------- | ----------- | --------- |
| event     | string  | Sim         | Identificador de recurso do evento |
| index     | integer | Sim         | Número do índice do conteúdo pai |

### Parâmetros de consulta

| Parâmetro   | Tipo   | Obrigatório | Descrição | Padrão/Valores |
| ----------- | ------ | ----------- | --------- | -------------- |
| language    | string | Não         | Código de idioma para conteúdo localizado | Padrão da plataforma |
| platform_id | string | Não         | UUID da plataforma para validação de propriedade | Derivado de X-PUBLIC-KEY |

> O endpoint aceita variações de nomes de parâmetros (`camelCase`, `kebab-case`, `CapitalCase`).

## Exemplos

### Exemplo de requisição (curl)

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <chave>" \
  -H "Accept-Language: pt-BR" \
  "https://sandbox.seu-dominio.com/api/v1/learn/events/nome-recurso-evento/contents/1/sub-contents?language=pt-BR"
```

### Exemplo de resposta

```json
{
  "data": [
    {
      "uuid": "ee0e8400-e29b-41d4-a716-446655440009",
      "index_number": 1,
      "parent_content_index": 1,
      "content_type": "video",
      "duration_minutes": 15,
      "title": {
        "language": "pt-BR",
        "value": "Fundamentos de HTML - Parte 1",
        "is_default": false
      },
      "description": {
        "language": "pt-BR",
        "value": "Introdução às tags e estrutura HTML"
      },
      "media": {
        "url": "https://example.com/sub-video-1.mp4",
        "thumbnail": "https://example.com/sub-thumb-1.jpg"
      }
    },
    {
      "uuid": "ff0e8400-e29b-41d4-a716-446655440010",
      "index_number": 2,
      "parent_content_index": 1,
      "content_type": "quiz",
      "duration_minutes": 5,
      "title": {
        "language": "pt-BR",
        "value": "Verificação de Conhecimento HTML"
      }
    }
  ]
}
```

## Estrutura JSON Explicada

| Campo                        | Tipo    | Descrição |
| ---------------------------- | ------- | --------- |
| data[]                       | array   | Lista de sub-conteúdos |
| data[].uuid                  | string  | Identificador único do sub-conteúdo |
| data[].index_number          | integer | Índice sequencial do sub-conteúdo |
| data[].parent_content_index  | integer | Número do índice do conteúdo pai |
| data[].content_type          | string  | Tipo de sub-conteúdo |
| data[].duration_minutes      | integer | Duração estimada em minutos |
| data[].title                 | object  | Título localizado do sub-conteúdo |
| data[].description           | object  | Descrição localizada do sub-conteúdo |
| data[].media                 | object  | URLs de mídia (quando aplicável) |

## Status HTTP

- 200: Sucesso
- 400: Requisição inválida
- 401: Não autenticado - Autenticação obrigatória
- 403: Proibido - A plataforma não possui este evento
- 404: Não encontrado - Evento ou conteúdo não encontrado
- 429: Limite de requests excedido
- 500: Erro interno

## Erros

```json
{
  "message": "Conteúdo não encontrado"
}
```

```json
{
  "message": "Não autenticado"
}
```

## Notas

- Retorna todos os sub-conteúdos para um conteúdo pai específico
- Requer autenticação, pois sub-conteúdos geralmente não são conteúdo de visualização
- O evento deve pertencer à plataforma que faz a requisição
- A propriedade da plataforma é validada usando o parâmetro `platform_id` ou derivado de `X-PUBLIC-KEY`
- Os sub-conteúdos são ordenados por `index_number`
- Títulos e descrições são localizados com base no parâmetro `language` ou cabeçalho `Accept-Language`
- Se o conteúdo localizado não estiver disponível, valores padrão são retornados

## Relacionados

- [Exibir Sub-Conteúdo do Evento](./EventContentSubContentShow.md)
- [Exibir Conteúdo do Evento](./EventContentShow.md)
- [Listar Conteúdos do Evento](./EventContentIndex.md)

## Changelog

- 2025-10-16: Documentação inicial
