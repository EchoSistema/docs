# Learn – Listar Visualizações de Eventos

## Endpoint

```
GET /api/v1/learn/v/events
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

### Parâmetros de consulta

| Parâmetro      | Tipo   | Obrigatório | Descrição | Padrão/Valores |
| -------------- | ------ | ----------- | --------- | -------------- |
| platform_uuid  | string | Não         | UUID da plataforma para filtragem | Derivado de X-PUBLIC-KEY |

> O endpoint aceita variações de nomes de parâmetros (`camelCase`, `kebab-case`, `CapitalCase`).

## Exemplos

### Exemplo de requisição (curl)

```bash
curl -X GET \
  -H "X-PUBLIC-KEY: <chave>" \
  -H "Accept-Language: pt-BR" \
  "https://sandbox.seu-dominio.com/api/v1/learn/v/events"
```

### Exemplo de resposta

```json
{
  "data": [
    {
      "_id": "507f1f77bcf86cd799439011",
      "event_uuid": "880e8400-e29b-41d4-a716-446655440003",
      "platform_uuid": "aa0e8400-e29b-41d4-a716-446655440005",
      "resource": "nome-recurso-evento",
      "title": "Curso Completo de Desenvolvimento Web",
      "description": "Aprenda desenvolvimento web do zero",
      "contents_count": 12,
      "total_duration_minutes": 540,
      "category": "Programação",
      "is_active": true,
      "created_at": "2025-01-15T10:30:00Z",
      "updated_at": "2025-10-10T14:20:00Z"
    },
    {
      "_id": "507f1f77bcf86cd799439012",
      "event_uuid": "990e8400-e29b-41d4-a716-446655440011",
      "platform_uuid": "aa0e8400-e29b-41d4-a716-446655440005",
      "resource": "outro-evento",
      "title": "JavaScript Avançado",
      "description": "Domine JavaScript moderno",
      "contents_count": 8,
      "total_duration_minutes": 360,
      "category": "Programação",
      "is_active": true,
      "created_at": "2025-02-20T08:00:00Z",
      "updated_at": "2025-09-15T16:45:00Z"
    }
  ]
}
```

## Estrutura JSON Explicada

| Campo                       | Tipo    | Descrição |
| --------------------------- | ------- | --------- |
| data[]                      | array   | Lista de views materializadas de eventos |
| data[]._id                  | string  | Identificador do documento MongoDB |
| data[].event_uuid           | string  | Identificador único do evento |
| data[].platform_uuid        | string  | UUID da plataforma associada |
| data[].resource             | string  | Nome do recurso do evento |
| data[].title                | string  | Título do evento |
| data[].description          | string  | Descrição do evento |
| data[].contents_count       | integer | Número de conteúdos |
| data[].total_duration_minutes | integer | Duração total em minutos |
| data[].category             | string  | Categoria do evento |
| data[].is_active            | boolean | Se o evento está ativo |
| data[].created_at           | string  | Data/hora de criação (ISO 8601) |
| data[].updated_at           | string  | Data/hora da última atualização (ISO 8601) |

## Status HTTP

- 200: Sucesso
- 400: Requisição inválida
- 401: Não autenticado
- 403: Proibido
- 404: Não encontrado
- 429: Limite de requests excedido
- 500: Erro interno

## Erros

```json
{
  "message": "Chave de plataforma inválida"
}
```

## Notas

- Retorna views materializadas de eventos do MongoDB para desempenho de leitura otimizado
- Views materializadas fornecem dados desnormalizados e pré-computados para consultas mais rápidas
- Os dados são lidos de um banco de dados NoSQL (MongoDB) separado do banco de dados relacional principal
- Os eventos são filtrados por `platform_uuid` que é derivado do cabeçalho `X-PUBLIC-KEY`
- Este endpoint é otimizado para operações de listagem e busca
- A view pode não refletir mudanças em tempo real; as atualizações são sincronizadas de forma assíncrona
- Útil para dashboards e relatórios onde o desempenho de leitura é crítico

## Relacionados

- [Listar Eventos](./EventIndex.md)
- [Exibir Evento](./EventShow.md)

## Changelog

- 2025-10-16: Documentação inicial
