# ReputationBook – Listar Reclamações (Backoffice)

## Endpoint

```
GET /api/v1/backoffice/complaints
```

## Descrição

Lista todas as reclamações do sistema com filtros avançados. Endpoint exclusivo para administradores do backoffice.

## Autenticação

Obrigatória – Bearer {token} com habilidade `backoffice`

## Cabeçalhos

| Cabeçalho        | Tipo   | Obrigatório | Descrição |
| ---------------- | ------ | ----------- | --------- |
| Authorization    | string | Sim         | Credencial `Bearer {token}`. |
| X-PUBLIC-KEY     | string | Sim         | Chave pública da plataforma. |
| Accept-Language  | string | Não         | Locale IETF (ex.: `pt-BR`, `en`, `es`). |

## Parâmetros de Query

| Parâmetro      | Tipo    | Obrigatório | Descrição |
| -------------- | ------- | ----------- | --------- |
| platform_id    | integer | Não         | Filtrar por ID da plataforma |
| company_id     | integer | Não         | Filtrar por ID da empresa |
| user_id        | integer | Não         | Filtrar por ID do usuário |
| status         | string  | Não         | Filtrar por status da reclamação |
| is_public      | boolean | Não         | Filtrar por visibilidade pública |
| language       | string  | Não         | Idioma dos títulos (padrão: pt-BR) |
| no_paginate    | boolean | Não         | Se `true`, retorna todos os resultados sem paginação |
| per_page       | integer | Não         | Número de registros por página (padrão: 15) |
| limit          | integer | Não         | Limite de resultados quando `no_paginate=true` |

## Exemplos

### Exemplo de requisição (curl)

```bash
# Listar todas as reclamações com paginação
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: pt-BR" \
  "https://sandbox.your-domain.com/api/v1/backoffice/complaints?per_page=15"

# Filtrar por status e plataforma
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  "https://sandbox.your-domain.com/api/v1/backoffice/complaints?status=pending&platform_id=1"

# Buscar apenas reclamações públicas
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  "https://sandbox.your-domain.com/api/v1/backoffice/complaints?is_public=true"
```

### Exemplo de resposta

```json
{
  "data": [
    {
      "uuid": "9d4e1c2a-3b4c-5d6e-7f8g-9h0i1j2k3l4m",
      "is_public": true,
      "status": "pending",
      "purchase_date": "2024-01-10T00:00:00Z",
      "invoice_number": "INV-2024-001",
      "created_at": "2024-01-15T10:30:00Z",
      "user": {
        "uuid": "8c3d0b1a-2e3f-4g5h-6i7j-8k9l0m1n2o3p",
        "name": "João Silva",
        "email": "joao.silva@example.com",
        "gender": "Masculino"
      },
      "platform": {
        "uuid": "7b2c9a0b-1d2e-3f4g-5h6i-7j8k9l0m1n2o",
        "name": "Plataforma Demo",
        "domain_area": {
          "uuid": "6a1b8c9d-0e1f-2g3h-4i5j-6k7l8m9n0o1p",
          "name": "E-commerce",
          "slug": "ecommerce"
        }
      },
      "company": {
        "uuid": "5a0b7c8d-9e0f-1g2h-3i4j-5k6l7m8n9o0p",
        "name": "Empresa XYZ Ltda",
        "slug": "empresa-xyz",
        "is_government": false
      },
      "companies_attendants_name": "Atendente Maria",
      "title": {
        "title": "Produto com defeito",
        "slug": "produto-com-defeito",
        "language": "pt-BR",
        "is_default": true
      },
      "meta": {
        "texts": []
      }
    }
  ],
  "links": {
    "first": "https://sandbox.your-domain.com/api/v1/backoffice/complaints?page=1",
    "last": "https://sandbox.your-domain.com/api/v1/backoffice/complaints?page=10",
    "prev": null,
    "next": "https://sandbox.your-domain.com/api/v1/backoffice/complaints?page=2"
  },
  "meta": {
    "current_page": 1,
    "from": 1,
    "last_page": 10,
    "per_page": 15,
    "to": 15,
    "total": 150
  }
}
```

## Estrutura JSON Explicada

### Campos Principais

| Campo | Tipo | Descrição |
| ----- | ---- | --------- |
| data | array | Lista de reclamações |
| data[].uuid | string | Identificador único da reclamação |
| data[].is_public | boolean | Se a reclamação é pública |
| data[].status | string | Status atual (pending, approved, replied, etc) |
| data[].purchase_date | string | Data da compra (ISO 8601) |
| data[].invoice_number | string | Número da nota fiscal |
| data[].created_at | string | Data de criação (ISO 8601) |

### Dados do Usuário

| Campo | Tipo | Descrição |
| ----- | ---- | --------- |
| data[].user | object | Dados do usuário que criou a reclamação |
| data[].user.uuid | string | UUID do usuário |
| data[].user.name | string | Nome do usuário |
| data[].user.email | string | Email do usuário |
| data[].user.gender | string | Gênero do usuário |

### Dados da Plataforma

| Campo | Tipo | Descrição |
| ----- | ---- | --------- |
| data[].platform | object | Dados da plataforma |
| data[].platform.uuid | string | UUID da plataforma |
| data[].platform.name | string | Nome da plataforma |
| data[].platform.domain_area | object | Área de domínio |
| data[].platform.domain_area.uuid | string | UUID da área |
| data[].platform.domain_area.name | string | Nome da área |
| data[].platform.domain_area.slug | string | Slug da área |

### Dados da Empresa (condicional)

| Campo | Tipo | Descrição |
| ----- | ---- | --------- |
| data[].company | object | Empresa reclamada (condicional) |
| data[].company.uuid | string | UUID da empresa |
| data[].company.name | string | Nome da empresa |
| data[].company.slug | string | Slug da empresa |
| data[].company.is_government | boolean | Se é empresa governamental |
| data[].companies_attendants_name | string | Nome do atendente responsável |

### Título (condicional)

| Campo | Tipo | Descrição |
| ----- | ---- | --------- |
| data[].title | object | Título da reclamação (condicional) |
| data[].title.title | string | Texto do título |
| data[].title.slug | string | Slug do título |
| data[].title.language | string | Idioma do título |
| data[].title.is_default | boolean | Se é o título padrão |

### Metadados

| Campo | Tipo | Descrição |
| ----- | ---- | --------- |
| data[].meta | object | Metadados adicionais |
| data[].meta.texts | array | Textos relacionados |

## Relações Carregadas

Este endpoint carrega automaticamente as seguintes relações:

- `user` - Usuário que criou a reclamação
- `platform` - Plataforma onde foi registrada
- `platform.domainArea` - Área de domínio da plataforma
- `company` - Empresa reclamada (quando disponível)
- `titles` - Títulos multilíngues da reclamação

## Filtros Disponíveis

O endpoint suporta os seguintes filtros através da classe `ComplaintFilter`:

- **platform_id**: Filtrar por plataforma específica
- **company_id**: Filtrar por empresa específica
- **user_id**: Filtrar por usuário específico
- **status**: Filtrar por status (pending, approved, replied, resolved, etc)
- **is_public**: Filtrar por visibilidade pública (true/false)
- **language**: Define o idioma dos títulos retornados

## Status Possíveis

- `initiated` - Reclamação iniciada
- `pending` - Aguardando análise
- `approved` - Aprovada
- `replied` - Respondida
- `amicably_resolved` - Resolvida amigavelmente
- `legally_resolved` - Resolvida judicialmente
- `unresolved` - Não resolvida
- `canceled` - Cancelada
- `archived` - Arquivada
- `spam` - Marcada como spam

## Status HTTP

- 200: OK
- 401: Não autenticado
- 403: Proibido (sem habilidade `backoffice`)
- 422: Erro de validação
- 429: Limite de requests excedido
- 500: Erro interno

## Erros

```json
{
  "message": "Mensagem de erro"
}
```

## Notas

- Este endpoint requer habilidade `backoffice` no token de autenticação
- Ordenação padrão: mais recentes primeiro
- A paginação padrão retorna 15 registros por página
- Use `no_paginate=true` com `limit` para obter todos os resultados sem paginação
- O parâmetro `language` afeta apenas os títulos, buscando primeiro pelo idioma especificado e depois pelo padrão
- Todos os emails e dados sensíveis são expostos (endpoint administrativo)
- As datas seguem o formato ISO 8601
- O campo `companies_attendants_name` só aparece quando há empresa associada

## Relacionados

- [Exibir Detalhes da Reclamação](BackofficeComplaintShow.md)
- [Complaint Resource](../Resources/ComplaintResource.md)

## Changelog

- 2025-10-26: Documentação inicial do endpoint de backoffice
