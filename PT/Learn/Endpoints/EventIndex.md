# Learn – Listar Eventos

## Endpoint

```
GET /api/v1/learn/events
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

| Parâmetro    | Tipo    | Obrigatório | Descrição | Padrão/Valores |
| ------------ | ------- | ----------- | --------- | -------------- |
| language     | string  | Não         | Código de idioma para títulos localizados | Padrão da plataforma |
| currency     | string  | Não         | Código de moeda para preços (ex.: USD, BRL, EUR) | Padrão da plataforma |
| per_page     | integer | Não         | Itens por página (paginação) | 10 |
| page         | integer | Não         | Número da página | 1 |
| no_paginate  | boolean | Não         | Retornar todos os resultados sem paginação | false |
| limit        | integer | Não         | Máximo de itens a retornar (quando no_paginate=true) | Sem limite |

> Parâmetros de filtro adicionais estão disponíveis através do EventFilter. O endpoint aceita variações de nomes de parâmetros (`camelCase`, `kebab-case`, `CapitalCase`).

## Exemplos

### Exemplo de requisição (curl)

```bash
curl -X GET \
  -H "X-PUBLIC-KEY: <chave>" \
  -H "Accept-Language: pt-BR" \
  "https://sandbox.seu-dominio.com/api/v1/learn/events?language=pt-BR&currency=BRL&per_page=10&page=1"
```

### Exemplo de resposta

```json
{
  "data": [
    {
      "uuid": "880e8400-e29b-41d4-a716-446655440003",
      "resource": "nome-recurso-evento",
      "contents_count": 12,
      "team_coordinators": [
        {
          "user_basic_data": {
            "uuid": "990e8400-e29b-41d4-a716-446655440004",
            "name": "João Silva",
            "images": [
              {
                "usage": "profile",
                "url": "https://example.com/profile.jpg"
              }
            ]
          }
        }
      ],
      "regional_information": {
        "country": "BR",
        "state": "SP"
      },
      "card": {
        "title": "Título do Card do Evento",
        "description": "Descrição do card do evento"
      },
      "titles": [
        {
          "language": "pt-BR",
          "value": "Título do Evento",
          "is_default": false
        }
      ],
      "prices": [
        {
          "currency_id": "BRL",
          "amount": 199.99,
          "is_default": false
        }
      ],
      "offer": {
        "is_active": true,
        "discount_percent": 20,
        "titles": [
          {
            "language": "pt-BR",
            "value": "Oferta Especial"
          }
        ]
      }
    }
  ],
  "links": {
    "first": "https://sandbox.seu-dominio.com/api/v1/learn/events?page=1",
    "last": "https://sandbox.seu-dominio.com/api/v1/learn/events?page=5",
    "prev": null,
    "next": "https://sandbox.seu-dominio.com/api/v1/learn/events?page=2"
  },
  "meta": {
    "current_page": 1,
    "from": 1,
    "last_page": 5,
    "path": "https://sandbox.seu-dominio.com/api/v1/learn/events",
    "per_page": 10,
    "to": 10,
    "total": 47
  }
}
```

## Estrutura JSON Explicada

| Campo                          | Tipo    | Descrição |
| ------------------------------ | ------- | --------- |
| data[]                         | array   | Lista de eventos |
| data[].uuid                    | string  | Identificador único do evento |
| data[].resource                | string  | Nome do recurso do evento |
| data[].contents_count          | integer | Número de conteúdos no evento |
| data[].team_coordinators[]     | array   | Lista de coordenadores da equipe |
| data[].regional_information    | object  | Informações regionais |
| data[].card                    | object  | Informações de exibição do card |
| data[].titles[]                | array   | Títulos localizados |
| data[].prices[]                | array   | Preços em diferentes moedas |
| data[].offer                   | object  | Informações de oferta ativa |
| links                          | object  | Links de paginação |
| meta                           | object  | Metadados de paginação |

## Status HTTP

- 200: Sucesso
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
  "message": "Chave de plataforma inválida",
  "errors": {
    "per_page": ["O per page deve ser um número inteiro."]
  }
}
```

## Notas

- Retorna todos os eventos para a plataforma autenticada
- Os resultados são paginados por padrão com 10 itens por página
- Use `no_paginate=true` para recuperar todos os resultados de uma vez (use com cautela)
- Títulos e preços são localizados com base nos parâmetros `language` e `currency`
- Se o conteúdo localizado não estiver disponível, valores padrão são retornados
- A resposta inclui coordenadores da equipe com suas imagens de perfil e biografias
- Ofertas ativas são incluídas na resposta quando disponíveis

## Relacionados

- [Exibir Evento](./EventShow.md)
- [Obter Contador de Eventos](./EventCounter.md)
- [Listar Conteúdos do Evento](./EventContentIndex.md)

## Changelog

- 2025-10-16: Documentação inicial
