# ReputationBook – Listar Reclamações Públicas

## Endpoint

```
GET /api/v1/complaints
```

Recupera uma lista paginada de reclamações públicas com opções abrangentes de filtragem.

## Autenticação

Nenhuma (endpoint público)

## Cabeçalhos

| Cabeçalho        | Tipo   | Obrigatório | Descrição |
| ---------------- | ------ | ----------- | --------- |
| X-PUBLIC-KEY     | string | Sim         | Chave pública da plataforma. |
| Accept-Language  | string | Não         | Locale IETF (ex.: `pt-BR`, `en`, `es`). Determina a localização de campos traduzíveis. |

## Parâmetros

### Parâmetros de consulta

| Parâmetro | Tipo    | Obrigatório | Descrição | Padrão/Valores |
| --------- | ------- | ----------- | --------- | -------------- |
| user      | string  | Não         | Filtrar por UUID, ID ou nome de usuário. | - |
| company   | string  | Não         | Filtrar por UUID, ID, nome, email ou URL da empresa. Aceita prefixos `email:` e `url:`. | - |
| company_name | string | Não | Filtrar por nome da empresa (correspondência parcial). | - |
| company_email | string | Não | Filtrar por email da empresa. | - |
| company_url | string | Não | Filtrar por URL do site da empresa. | - |
| status    | string  | Não         | Filtrar por status da reclamação (ex.: `pending`, `in_progress`, `resolved`, `rejected`). | - |
| is_public | boolean | Não         | Filtrar por visibilidade pública. Automaticamente definido como `true` para este endpoint. | true |
| title     | string  | Não         | Filtrar por título da reclamação (correspondência parcial). | - |
| complaint | string  | Não         | Filtrar por texto da reclamação (correspondência parcial). | - |
| complaint_language | string | Não | Filtrar reclamações por código de idioma do texto. | - |
| invoice   | string  | Não         | Filtrar por número da fatura. | - |
| created_at | date   | Não         | Filtrar por data exata de criação. | - |
| created_at_period | array | Não | Filtrar por intervalo de datas `[data_inicio, data_fim]`. | - |
| purchase_date | date | Não | Filtrar por data exata de compra. | - |
| purchase_date_period | array | Não | Filtrar por intervalo de datas de compra `[data_inicio, data_fim]`. | - |
| month     | integer | Não         | Filtrar por mês (1-12). | - |
| year      | integer | Não         | Filtrar por ano (1990-2100). Obrigatório se `month` for fornecido. | - |
| with_texts | boolean | Não | Incluir conteúdo de texto da reclamação na resposta. | false |
| per_page  | integer | Não         | Itens por página para paginação. | 15 |
| no_paginate | boolean | Não | Quando `true`, desativa a paginação e retorna todos os resultados (limitado por `limit`). | false |
| limit     | integer | Não         | Número máximo de resultados quando `no_paginate` é true. | - |

> Todos os parâmetros de filtro aceitam variantes `camelCase`, `kebab-case` e `snake_case`.

## Exemplos

### Exemplo de requisição (curl)

```bash
curl -X GET \
  -H "X-PUBLIC-KEY: <chave>" \
  -H "Accept-Language: pt-BR" \
  "https://sandbox.seu-dominio.com/api/v1/complaints?status=pending&per_page=10"
```

### Exemplo de resposta

```json
{
  "data": [
    {
      "uuid": "9d1234ab-5678-90ef-1234-567890abcdef",
      "user": {
        "uuid": "9a1234ab-5678-90ef-1234-567890abcdef",
        "name": "João Silva",
        "gender": "male"
      },
      "platform": {
        "uuid": "9b1234ab-5678-90ef-1234-567890abcdef",
        "name": "Nome da Plataforma",
        "domain_area": {
          "uuid": "9c1234ab-5678-90ef-1234-567890abcdef",
          "name": "Área de Serviço",
          "slug": "area-servico"
        }
      },
      "company": {
        "uuid": "9e1234ab-5678-90ef-1234-567890abcdef",
        "name": "Empresa ABC",
        "slug": "empresa-abc"
      },
      "titles": [
        {
          "content": "Problema de qualidade do produto",
          "language": "pt-BR",
          "is_default": true
        }
      ],
      "status": "pending",
      "is_public": true,
      "purchase_date": "2025-10-01",
      "created_at": "2025-10-15T14:30:00.000000Z",
      "updated_at": "2025-10-15T14:30:00.000000Z"
    }
  ],
  "links": {
    "first": "https://api.example.com/api/v1/complaints?page=1",
    "last": "https://api.example.com/api/v1/complaints?page=5",
    "prev": null,
    "next": "https://api.example.com/api/v1/complaints?page=2"
  },
  "meta": {
    "current_page": 1,
    "from": 1,
    "last_page": 5,
    "path": "https://api.example.com/api/v1/complaints",
    "per_page": 15,
    "to": 15,
    "total": 72
  }
}
```

## Estrutura JSON Explicada

| Campo       | Tipo    | Descrição |
| ----------- | ------- | --------- |
| data[]      | array   | Lista de objetos de reclamações. |
| data[].uuid | uuid    | Identificador único da reclamação. |
| data[].user | object  | Usuário que criou a reclamação. |
| data[].platform | object | Plataforma onde a reclamação foi registrada. |
| data[].company | object | Empresa sobre a qual se reclama. |
| data[].titles[] | array | Títulos de reclamação localizados. |
| data[].status | string | Status atual da reclamação. |
| data[].is_public | boolean | Se a reclamação é visível publicamente. |
| data[].purchase_date | date | Data de compra relacionada à reclamação. |
| data[].created_at | string | Marca de tempo de criação da reclamação (ISO-8601). |
| data[].updated_at | string | Marca de tempo de última atualização da reclamação (ISO-8601). |
| links       | object  | Links de paginação. |
| meta        | object  | Metadados de paginação. |

## Status HTTP

- 200: Sucesso
- 400: Requisição inválida
- 401: Não autenticado
- 422: Erro de validação
- 429: Limite de requests excedido
- 500: Erro interno

## Erros

```json
{
  "message": "Erro de validação",
  "errors": {
    "status": ["O status selecionado é inválido."]
  }
}
```

## Notas

- Este endpoint filtra automaticamente para mostrar apenas reclamações públicas (`is_public=true`).
- O array `titles` é filtrado pelo cabeçalho `Accept-Language` ou retorna o título padrão.
- Quando `with_texts=true`, o conteúdo de texto da reclamação é incluído na resposta.
- A paginação está habilitada por padrão com 15 itens por página.
- Filtros de string realizam correspondência parcial sem distinção de maiúsculas e minúsculas.

## Relacionados

- [Mostrar Reclamação](ComplaintShow.md)
- [Criar Reclamação](ComplaintStore.md)
- [Status de Reclamações](ComplaintStatus.md)
