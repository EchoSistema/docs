# Learn – Listar Conteúdos do Evento

## Endpoint

```
GET /api/v1/learn/events/{event}/contents
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

| Parâmetro | Tipo   | Obrigatório | Descrição |
| --------- | ------ | ----------- | --------- |
| event     | string | Sim         | Identificador de recurso do evento |

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
  "https://sandbox.seu-dominio.com/api/v1/learn/events/nome-recurso-evento/contents?language=pt-BR"
```

### Exemplo de resposta

```json
{
  "data": [
    {
      "uuid": "cc0e8400-e29b-41d4-a716-446655440007",
      "index_number": 1,
      "content_type": "video",
      "duration_minutes": 45,
      "is_preview": false,
      "title": {
        "language": "pt-BR",
        "value": "Introdução ao Desenvolvimento Web",
        "is_default": false
      },
      "description": {
        "language": "pt-BR",
        "value": "Aprenda os fundamentos do desenvolvimento web..."
      },
      "media": {
        "url": "https://example.com/video.mp4",
        "thumbnail": "https://example.com/thumbnail.jpg"
      },
      "sub_contents_count": 3
    },
    {
      "uuid": "dd0e8400-e29b-41d4-a716-446655440008",
      "index_number": 2,
      "content_type": "text",
      "duration_minutes": 15,
      "is_preview": true,
      "title": {
        "language": "pt-BR",
        "value": "Fundamentos de HTML"
      },
      "sub_contents_count": 0
    }
  ]
}
```

## Estrutura JSON Explicada

| Campo                      | Tipo    | Descrição |
| -------------------------- | ------- | --------- |
| data[]                     | array   | Lista de conteúdos do evento |
| data[].uuid                | string  | Identificador único do conteúdo |
| data[].index_number        | integer | Índice sequencial do conteúdo |
| data[].content_type        | string  | Tipo de conteúdo (video, text, pdf, etc.) |
| data[].duration_minutes    | integer | Duração estimada em minutos |
| data[].is_preview          | boolean | Se este conteúdo está disponível como visualização |
| data[].title               | object  | Título localizado do conteúdo |
| data[].description         | object  | Descrição localizada do conteúdo |
| data[].media               | object  | URLs de mídia (quando aplicável) |
| data[].sub_contents_count  | integer | Número de sub-conteúdos |

## Status HTTP

- 200: Sucesso
- 400: Requisição inválida
- 401: Não autenticado
- 403: Proibido
- 404: Não encontrado - Evento não encontrado
- 429: Limite de requests excedido
- 500: Erro interno

## Erros

```json
{
  "message": "Evento não encontrado"
}
```

## Notas

- Retorna todos os conteúdos para um evento específico
- Os conteúdos são ordenados por `index_number`
- Títulos e descrições de conteúdo são localizados com base no parâmetro `language` ou cabeçalho `Accept-Language`
- Se o conteúdo localizado não estiver disponível, valores padrão são retornados
- O campo `sub_contents_count` indica se o conteúdo possui sub-conteúdos aninhados
- Conteúdos de visualização (`is_preview: true`) geralmente estão disponíveis sem autenticação

## Relacionados

- [Exibir Conteúdo do Evento](./EventContentShow.md)
- [Listar Sub-Conteúdos do Evento](./EventContentSubContentsIndex.md)
- [Exibir Evento](./EventShow.md)

## Changelog

- 2025-10-16: Documentação inicial
