# Learn – Exibir Conteúdo do Evento

## Endpoint

```
GET /api/v1/learn/events/{event}/contents/{index}
```

## Autenticação

Nenhuma

## Cabeçalhos

| Cabeçalho       | Tipo   | Obrigatório | Descrição |
| --------------- | ------ | ----------- | --------- |
| Authorization   | string | Não         | `Bearer {token}` (opcional). |
| X-PUBLIC-KEY    | string | Sim         | Chave pública da plataforma. |
| Accept-Language | string | Não         | Locale IETF (ex.: `pt-BR`, `en`, `es`). |

## Parâmetros

### Parâmetros de caminho

| Parâmetro | Tipo    | Obrigatório | Descrição |
| --------- | ------- | ----------- | --------- |
| event     | string  | Sim         | Identificador de recurso do evento |
| index     | integer | Sim         | Número do índice do conteúdo |

### Parâmetros de consulta

| Parâmetro | Tipo   | Obrigatório | Descrição | Padrão/Valores |
| --------- | ------ | ----------- | --------- | -------------- |
| language  | string | Não         | Código de idioma para conteúdo localizado | Padrão da plataforma |

> O endpoint aceita variações de nomes de parâmetros (`camelCase`, `kebab-case`, `CapitalCase`).

## Exemplos

### Exemplo de requisição (curl)

```bash
curl -X GET \
  -H "X-PUBLIC-KEY: <chave>" \
  -H "Accept-Language: pt-BR" \
  "https://sandbox.seu-dominio.com/api/v1/learn/events/nome-recurso-evento/contents/1?language=pt-BR"
```

### Exemplo de resposta

```json
{
  "data": {
    "uuid": "cc0e8400-e29b-41d4-a716-446655440007",
    "index_number": 1,
    "content_type": "video",
    "duration_minutes": 45,
    "is_preview": false,
    "event_title": {
      "language": "pt-BR",
      "value": "Curso Completo de Desenvolvimento Web"
    },
    "title": {
      "language": "pt-BR",
      "value": "Introdução ao Desenvolvimento Web",
      "is_default": false
    },
    "description": {
      "language": "pt-BR",
      "value": "Aprenda os fundamentos do desenvolvimento web incluindo HTML, CSS e conceitos básicos de JavaScript."
    },
    "media": {
      "type": "video",
      "url": "https://example.com/video.mp4",
      "thumbnail": "https://example.com/thumbnail.jpg",
      "duration_seconds": 2700
    },
    "attachments": [
      {
        "name": "Material do Curso.pdf",
        "url": "https://example.com/material.pdf",
        "size_bytes": 1048576
      }
    ],
    "sub_contents_count": 3,
    "completed": false,
    "progress_percent": 0
  }
}
```

## Estrutura JSON Explicada

| Campo                     | Tipo    | Descrição |
| ------------------------- | ------- | --------- |
| data                      | object  | Detalhes do conteúdo |
| data.uuid                 | string  | Identificador único do conteúdo |
| data.index_number         | integer | Índice sequencial do conteúdo |
| data.content_type         | string  | Tipo de conteúdo |
| data.duration_minutes     | integer | Duração estimada em minutos |
| data.is_preview           | boolean | Se este é um conteúdo de visualização |
| data.event_title          | object  | Título localizado do evento pai |
| data.title                | object  | Título localizado do conteúdo |
| data.description          | object  | Descrição localizada do conteúdo |
| data.media                | object  | Informações de mídia |
| data.attachments[]        | array   | Anexos para download |
| data.sub_contents_count   | integer | Número de sub-conteúdos |
| data.completed            | boolean | Status de conclusão do usuário (quando autenticado) |
| data.progress_percent     | integer | Porcentagem de progresso do usuário (quando autenticado) |

## Status HTTP

- 200: Sucesso
- 400: Requisição inválida
- 401: Não autenticado
- 403: Proibido
- 404: Não encontrado - Evento ou conteúdo não encontrado
- 429: Limite de requests excedido
- 500: Erro interno

## Erros

```json
{
  "message": "Conteúdo não encontrado"
}
```

## Notas

- Retorna informações detalhadas para um conteúdo específico do evento por seu número de índice
- O conteúdo é identificado por seu `index_number` dentro do evento
- Inclui o título do evento pai para contexto
- Títulos e descrições de conteúdo são localizados com base no parâmetro `language` ou cabeçalho `Accept-Language`
- Se o conteúdo localizado não estiver disponível, valores padrão são retornados
- Quando o usuário está autenticado, inclui status de conclusão e informações de progresso
- Conteúdos de visualização são acessíveis sem autenticação

## Relacionados

- [Listar Conteúdos do Evento](./EventContentIndex.md)
- [Listar Sub-Conteúdos do Evento](./EventContentSubContentsIndex.md)
- [Exibir Evento](./EventShow.md)

## Changelog

- 2025-10-16: Documentação inicial
