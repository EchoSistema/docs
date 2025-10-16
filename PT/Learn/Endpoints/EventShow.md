# Learn – Exibir Evento

## Endpoint

```
GET /api/v1/learn/events/{event}
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

| Parâmetro   | Tipo   | Obrigatório | Descrição | Padrão/Valores |
| ----------- | ------ | ----------- | --------- | -------------- |
| platform_id | string | Não         | UUID da plataforma para validação de propriedade | Derivado de X-PUBLIC-KEY |

> O parâmetro `event` usa vinculação de modelo de rota com o campo `resource`. O endpoint aceita variações de nomes de parâmetros (`camelCase`, `kebab-case`, `CapitalCase`).

## Exemplos

### Exemplo de requisição (curl)

```bash
curl -X GET \
  -H "X-PUBLIC-KEY: <chave>" \
  -H "Accept-Language: pt-BR" \
  "https://sandbox.seu-dominio.com/api/v1/learn/events/nome-recurso-evento"
```

### Exemplo de resposta

```json
{
  "data": {
    "uuid": "880e8400-e29b-41d4-a716-446655440003",
    "resource": "nome-recurso-evento",
    "platform_uuid": "aa0e8400-e29b-41d4-a716-446655440005",
    "contents_count": 12,
    "team_coordinators": [
      {
        "user_basic_data": {
          "uuid": "990e8400-e29b-41d4-a716-446655440004",
          "name": "João Silva",
          "email": "joao@example.com",
          "images": [
            {
              "usage": "profile",
              "url": "https://example.com/profile.jpg"
            }
          ],
          "biography": {
            "job_title": {
              "title": "Instrutor Sênior"
            },
            "user": {
              "social_medias": [
                {
                  "platform": "linkedin",
                  "url": "https://linkedin.com/in/joaosilva"
                }
              ]
            }
          }
        }
      }
    ],
    "regional_information": {
      "country": "BR",
      "state": "SP",
      "city": "São Paulo"
    },
    "card": {
      "title": "Título do Card do Evento",
      "description": "Descrição do card do evento",
      "image_url": "https://example.com/card.jpg"
    },
    "titles": [
      {
        "language": "pt-BR",
        "value": "Curso Completo de Desenvolvimento Web",
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
      "valid_until": "2025-12-31T23:59:59Z",
      "titles": [
        {
          "language": "pt-BR",
          "value": "Oferta por Tempo Limitado"
        }
      ]
    }
  }
}
```

## Estrutura JSON Explicada

| Campo                               | Tipo    | Descrição |
| ----------------------------------- | ------- | --------- |
| data                                | object  | Detalhes do evento |
| data.uuid                           | string  | Identificador único do evento |
| data.resource                       | string  | Nome do recurso do evento |
| data.platform_uuid                  | string  | UUID da plataforma associada |
| data.contents_count                 | integer | Número de conteúdos no evento |
| data.team_coordinators[]            | array   | Lista de coordenadores da equipe com detalhes completos |
| data.team_coordinators[].user_basic_data | object | Informações do usuário coordenador |
| data.regional_information           | object  | Informações regionais |
| data.card                           | object  | Informações de exibição do card |
| data.titles[]                       | array   | Títulos localizados |
| data.prices[]                       | array   | Preços em diferentes moedas |
| data.offer                          | object  | Informações de oferta ativa (quando disponível) |

## Status HTTP

- 200: Sucesso
- 400: Requisição inválida
- 401: Não autenticado
- 403: Proibido - A plataforma não possui este evento
- 404: Não encontrado - Evento não encontrado
- 429: Limite de requests excedido
- 500: Erro interno

## Erros

```json
{
  "message": "Evento não encontrado"
}
```

```json
{
  "message": "Este evento não pertence à plataforma especificada"
}
```

## Notas

- Retorna informações detalhadas para um evento específico
- O evento deve pertencer à plataforma que faz a requisição
- A propriedade da plataforma é validada usando o parâmetro `platform_id` ou derivado de `X-PUBLIC-KEY`
- Inclui informações completas dos coordenadores da equipe com imagens e biografias
- O conteúdo localizado é retornado com base no cabeçalho `Accept-Language`
- A resposta inclui ofertas ativas quando disponíveis

## Relacionados

- [Listar Eventos](./EventIndex.md)
- [Obter Contador de Eventos](./EventCounter.md)
- [Obter Texto do Evento](./EventText.md)
- [Listar Conteúdos do Evento](./EventContentIndex.md)

## Changelog

- 2025-10-16: Documentação inicial
