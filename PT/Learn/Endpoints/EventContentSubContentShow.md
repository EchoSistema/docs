# Learn – Exibir Sub-Conteúdo do Evento

## Endpoint

```
GET /api/v1/learn/events/{event}/contents/{index}/sub-contents/{subIndex}
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
| subIndex  | integer | Sim         | Número do índice do sub-conteúdo |

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
  "https://sandbox.seu-dominio.com/api/v1/learn/events/nome-recurso-evento/contents/1/sub-contents/1?language=pt-BR"
```

### Exemplo de resposta

```json
{
  "data": {
    "uuid": "ee0e8400-e29b-41d4-a716-446655440009",
    "index_number": 1,
    "parent_content": {
      "uuid": "cc0e8400-e29b-41d4-a716-446655440007",
      "index_number": 1,
      "title": {
        "language": "pt-BR",
        "value": "Introdução ao Desenvolvimento Web"
      }
    },
    "content_type": "video",
    "duration_minutes": 15,
    "title": {
      "language": "pt-BR",
      "value": "Fundamentos de HTML - Parte 1",
      "is_default": false
    },
    "description": {
      "language": "pt-BR",
      "value": "Aprenda sobre tags HTML, elementos e estrutura de documentos. Esta lição cobre os blocos básicos de construção do HTML."
    },
    "media": {
      "type": "video",
      "url": "https://example.com/sub-video-1.mp4",
      "thumbnail": "https://example.com/sub-thumb-1.jpg",
      "duration_seconds": 900
    },
    "attachments": [
      {
        "name": "Guia Rápido HTML.pdf",
        "url": "https://example.com/html-cheatsheet.pdf",
        "size_bytes": 512000
      }
    ],
    "completed": false,
    "progress_percent": 0
  }
}
```

## Estrutura JSON Explicada

| Campo                        | Tipo    | Descrição |
| ---------------------------- | ------- | --------- |
| data                         | object  | Detalhes do sub-conteúdo |
| data.uuid                    | string  | Identificador único do sub-conteúdo |
| data.index_number            | integer | Índice sequencial do sub-conteúdo |
| data.parent_content          | object  | Informações do conteúdo pai |
| data.parent_content.index_number | integer | Índice do conteúdo pai |
| data.parent_content.title    | object  | Título localizado do conteúdo pai |
| data.content_type            | string  | Tipo de sub-conteúdo |
| data.duration_minutes        | integer | Duração estimada em minutos |
| data.title                   | object  | Título localizado do sub-conteúdo |
| data.description             | object  | Descrição localizada do sub-conteúdo |
| data.media                   | object  | Informações de mídia |
| data.attachments[]           | array   | Anexos para download |
| data.completed               | boolean | Status de conclusão do usuário |
| data.progress_percent        | integer | Porcentagem de progresso do usuário |

## Status HTTP

- 200: Sucesso
- 400: Requisição inválida
- 401: Não autenticado - Autenticação obrigatória
- 403: Proibido - A plataforma não possui este evento
- 404: Não encontrado - Evento, conteúdo ou sub-conteúdo não encontrado
- 429: Limite de requests excedido
- 500: Erro interno

## Erros

```json
{
  "message": "Sub-conteúdo não encontrado"
}
```

```json
{
  "message": "Não autenticado"
}
```

## Notas

- Retorna informações detalhadas para um sub-conteúdo específico por seu número de índice
- Requer autenticação, pois sub-conteúdos geralmente não são conteúdo de visualização
- O evento deve pertencer à plataforma que faz a requisição
- A propriedade da plataforma é validada usando o parâmetro `platform_id` ou derivado de `X-PUBLIC-KEY`
- Inclui as informações do conteúdo pai para contexto
- Títulos e descrições são localizados com base no parâmetro `language` ou cabeçalho `Accept-Language`
- Se o conteúdo localizado não estiver disponível, valores padrão são retornados
- Inclui status de conclusão e informações de progresso para o usuário autenticado

## Relacionados

- [Listar Sub-Conteúdos do Evento](./EventContentSubContentsIndex.md)
- [Exibir Conteúdo do Evento](./EventContentShow.md)
- [Listar Conteúdos do Evento](./EventContentIndex.md)

## Changelog

- 2025-10-16: Documentação inicial
