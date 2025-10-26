# ReputationBook – Exibir Detalhes da Reclamação (Backoffice)

## Endpoint

```
GET /api/v1/backoffice/complaints/{complaint}
```

## Descrição

Retorna informações detalhadas de uma reclamação específica, incluindo textos completos, imagens, respostas, histórico de mudanças de status e todos os dados relacionados. Endpoint exclusivo para administradores do backoffice.

## Autenticação

Obrigatória – Bearer {token} com habilidade `backoffice`

## Cabeçalhos

| Cabeçalho        | Tipo   | Obrigatório | Descrição |
| ---------------- | ------ | ----------- | --------- |
| Authorization    | string | Sim         | Credencial `Bearer {token}`. |
| X-PUBLIC-KEY     | string | Sim         | Chave pública da plataforma. |
| Accept-Language  | string | Não         | Locale IETF (ex.: `pt-BR`, `en`, `es`). |

## Parâmetros de URL

| Parâmetro  | Tipo   | Obrigatório | Descrição |
| ---------- | ------ | ----------- | --------- |
| complaint  | string | Sim         | UUID da reclamação a ser exibida |

## Exemplos

### Exemplo de requisição (curl)

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: pt-BR" \
  "https://sandbox.your-domain.com/api/v1/backoffice/complaints/9d4e1c2a-3b4c-5d6e-7f8g-9h0i1j2k3l4m"
```

### Exemplo de resposta

```json
{
  "uuid": "9d4e1c2a-3b4c-5d6e-7f8g-9h0i1j2k3l4m",
  "is_public": true,
  "status": "replied",
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
    },
    "company": {
      "name": "Empresa Plataforma Ltda",
      "slug": "empresa-plataforma",
      "is_governamental": false
    }
  },
  "company": {
    "uuid": "5a0b7c8d-9e0f-1g2h-3i4j-5k6l7m8n9o0p",
    "name": "Empresa XYZ Ltda",
    "slug": "empresa-xyz",
    "is_government": false,
    "cnpj": "12.345.678/0001-90",
    "addresses": [
      {
        "zipcode": "01310-100",
        "street": "Av. Paulista",
        "number": "1000",
        "city": "São Paulo",
        "state": "SP",
        "country": "Brasil"
      }
    ],
    "contacts": [
      {
        "type": "phone",
        "value": "+55 11 98765-4321"
      },
      {
        "type": "email",
        "value": "contato@empresaxyz.com"
      }
    ],
    "social_medias": [
      {
        "platform": "Facebook",
        "url": "https://facebook.com/empresaxyz"
      }
    ],
    "rating": {
      "average": 4.5,
      "total_reviews": 150
    },
    "fiscal_documents": [
      {
        "type": "CNPJ",
        "number": "12.345.678/0001-90"
      }
    ]
  },
  "title": {
    "title": "Produto com defeito e péssimo atendimento",
    "slug": "produto-com-defeito-e-pessimo-atendimento",
    "language": "pt-BR",
    "is_default": true
  },
  "texts": [
    {
      "content": "<p>Comprei o produto em 10/01/2024 e ao chegar em casa percebi que estava com defeito...</p>",
      "usage": "complaint_text",
      "language": "pt-BR",
      "is_default": true,
      "created_at": "2024-01-15T10:30:00Z"
    }
  ],
  "description": {
    "content": "<p>Descrição detalhada do problema com o produto...</p>",
    "usage": "complaint_description",
    "language": "pt-BR",
    "is_default": true,
    "created_at": "2024-01-15T10:35:00Z"
  },
  "images": [
    {
      "uuid": "4a9b6c7d-8e9f-0g1h-2i3j-4k5l6m7n8o9p",
      "usage": "evidence",
      "url": "https://cdn.example.com/complaints/evidence1.jpg",
      "created_at": "2024-01-15T10:40:00Z"
    },
    {
      "uuid": "3a8b5c6d-7e8f-9g0h-1i2j-3k4l5m6n7o8p",
      "usage": "product_photo",
      "url": "https://cdn.example.com/complaints/product.jpg",
      "created_at": "2024-01-15T10:41:00Z"
    }
  ],
  "replies": [
    {
      "uuid": "2a7b4c5d-6e7f-8g9h-0i1j-2k3l4m5n6o7p",
      "is_support": false,
      "index_number": 1,
      "created_at": "2024-01-16T14:20:00Z",
      "user": {
        "uuid": "1a6b3c4d-5e6f-7g8h-9i0j-1k2l3m4n5o6p",
        "name": "Atendente Maria",
        "email": "maria@empresaxyz.com"
      },
      "text": {
        "content": "Olá, lamentamos o ocorrido. Vamos providenciar a troca do produto.",
        "language": "pt-BR",
        "is_default": true,
        "usage": "reply_text"
      }
    },
    {
      "uuid": "0a5b2c3d-4e5f-6g7h-8i9j-0k1l2m3n4o5p",
      "is_support": true,
      "index_number": 2,
      "created_at": "2024-01-17T09:15:00Z",
      "user": {
        "uuid": "9a4b1c2d-3e4f-5g6h-7i8j-9k0l1m2n3o4p",
        "name": "Suporte EchoSistema",
        "email": "suporte@echosistema.com"
      },
      "text": {
        "content": "Acompanhamos o caso e verificamos que a empresa está tomando as providências.",
        "language": "pt-BR",
        "is_default": true,
        "usage": "reply_text"
      }
    }
  ],
  "status_change_history": [
    {
      "previous_status": null,
      "status": "initiated",
      "updated_by": null,
      "changed_at": "2024-01-15T10:30:00Z"
    },
    {
      "previous_status": "initiated",
      "status": "pending",
      "updated_by": "Admin João",
      "changed_at": "2024-01-15T11:00:00Z"
    },
    {
      "previous_status": "pending",
      "status": "approved",
      "updated_by": "Admin Maria",
      "changed_at": "2024-01-15T14:00:00Z"
    },
    {
      "previous_status": "approved",
      "status": "replied",
      "updated_by": null,
      "changed_at": "2024-01-16T14:20:00Z"
    }
  ]
}
```

## Estrutura JSON Explicada

### Campos Principais (sempre presentes)

| Campo | Tipo | Descrição |
| ----- | ---- | --------- |
| uuid | string | Identificador único da reclamação |
| is_public | boolean | Se a reclamação é visível publicamente |
| status | string | Status atual da reclamação |
| purchase_date | string | Data da compra relacionada (ISO 8601) |
| invoice_number | string | Número da nota fiscal |
| created_at | string | Data de criação da reclamação (ISO 8601) |

### Dados do Usuário (condicional)

| Campo | Tipo | Descrição |
| ----- | ---- | --------- |
| user | object | Dados completos do usuário (condicional) |
| user.uuid | string | UUID do usuário |
| user.name | string | Nome completo |
| user.email | string | Email do usuário |
| user.gender | string | Gênero (Masculino, Feminino, etc) |

### Dados da Plataforma (condicional)

| Campo | Tipo | Descrição |
| ----- | ---- | --------- |
| platform | object | Dados da plataforma (condicional) |
| platform.uuid | string | UUID da plataforma |
| platform.name | string | Nome da plataforma |
| platform.domain_area | object | Área de domínio |
| platform.domain_area.uuid | string | UUID da área |
| platform.domain_area.name | string | Nome da área |
| platform.domain_area.slug | string | Slug da área |
| platform.company | object | Empresa da plataforma (condicional) |
| platform.company.name | string | Nome da empresa |
| platform.company.slug | string | Slug da empresa |
| platform.company.is_governamental | boolean | Se é governamental |

### Dados da Empresa Reclamada (condicional)

| Campo | Tipo | Descrição |
| ----- | ---- | --------- |
| company | object | Empresa reclamada (via ClaimedCompanyResource) |
| company.uuid | string | UUID da empresa |
| company.name | string | Nome da empresa |
| company.slug | string | Slug da empresa |
| company.is_government | boolean | Se é governamental |
| company.cnpj | string | CNPJ da empresa |
| company.addresses | array | Endereços da empresa |
| company.contacts | array | Contatos da empresa |
| company.social_medias | array | Redes sociais |
| company.rating | object | Avaliação da empresa |
| company.fiscal_documents | array | Documentos fiscais |

### Título (condicional)

| Campo | Tipo | Descrição |
| ----- | ---- | --------- |
| title | object | Título principal da reclamação (condicional) |
| title.title | string | Texto do título |
| title.slug | string | Slug do título |
| title.language | string | Idioma (pt-BR, en, es) |
| title.is_default | boolean | Se é o título padrão |

### Textos (condicional)

| Campo | Tipo | Descrição |
| ----- | ---- | --------- |
| texts | array | Lista de textos da reclamação (condicional) |
| texts[].content | string | Conteúdo HTML do texto |
| texts[].usage | string | Tipo de uso (complaint_text, etc) |
| texts[].language | string | Idioma do texto |
| texts[].is_default | boolean | Se é o texto padrão |
| texts[].created_at | string | Data de criação (ISO 8601) |

### Descrição (condicional)

| Campo | Tipo | Descrição |
| ----- | ---- | --------- |
| description | object | Descrição detalhada (condicional) |
| description.content | string | Conteúdo HTML |
| description.usage | string | Tipo de uso |
| description.language | string | Idioma |
| description.is_default | boolean | Se é padrão |
| description.created_at | string | Data de criação (ISO 8601) |

### Imagens (condicional)

| Campo | Tipo | Descrição |
| ----- | ---- | --------- |
| images | array | Lista de imagens anexadas (condicional) |
| images[].uuid | string | UUID da imagem |
| images[].usage | string | Uso da imagem (evidence, product_photo, etc) |
| images[].url | string | URL completa da imagem |
| images[].created_at | string | Data de upload (ISO 8601) |

### Respostas (condicional)

| Campo | Tipo | Descrição |
| ----- | ---- | --------- |
| replies | array | Lista de respostas à reclamação (condicional) |
| replies[].uuid | string | UUID da resposta |
| replies[].is_support | boolean | Se é resposta do suporte |
| replies[].index_number | integer | Número sequencial da resposta |
| replies[].created_at | string | Data da resposta (ISO 8601) |
| replies[].user | object | Usuário que respondeu |
| replies[].user.uuid | string | UUID do usuário |
| replies[].user.name | string | Nome do usuário |
| replies[].user.email | string | Email do usuário |
| replies[].text | object | Conteúdo da resposta |
| replies[].text.content | string | Texto da resposta |
| replies[].text.language | string | Idioma |
| replies[].text.is_default | boolean | Se é padrão |
| replies[].text.usage | string | Tipo de uso |

### Histórico de Mudanças de Status

| Campo | Tipo | Descrição |
| ----- | ---- | --------- |
| status_change_history | array | Histórico completo de mudanças (condicional) |
| status_change_history[].previous_status | string\|null | Status anterior (null na criação) |
| status_change_history[].status | string | Novo status |
| status_change_history[].updated_by | string\|null | Nome de quem alterou (null se foi o próprio usuário) |
| status_change_history[].changed_at | string | Data da mudança (ISO 8601) |

## Relações Carregadas

Este endpoint carrega automaticamente as seguintes relações conforme definido em `Complaint::RESOURCE_RELATIONS`:

- `user` - Usuário que criou a reclamação
- `platform` - Plataforma onde foi registrada
- `platform.domainArea` - Área de domínio da plataforma
- `company` - Empresa reclamada
- `company.addresses` - Endereços da empresa
- `company.addresses.country` - País dos endereços
- `company.addresses.state` - Estado dos endereços
- `company.addresses.city` - Cidade dos endereços
- `company.contacts` - Contatos da empresa
- `company.socialMedias` - Redes sociais
- `company.rating` - Avaliação da empresa
- `company.fiscalDocuments` - Documentos fiscais
- `titles` - Títulos multilíngues
- `description` - Descrição detalhada
- `images` - Imagens anexadas
- `replies` - Respostas à reclamação
- `replies.user` - Usuários que responderam
- `statusChangeEvents` - Histórico de mudanças
- `statusChangeEvents.user` - Usuários que alteraram status

## Status HTTP

- 200: OK
- 401: Não autenticado
- 403: Proibido (sem habilidade `backoffice`)
- 404: Reclamação não encontrada
- 422: Erro de validação
- 429: Limite de requests excedido
- 500: Erro interno

## Erros

```json
{
  "message": "Mensagem de erro"
}
```

## Campos Condicionais

Os seguintes blocos de campos são condicionais e só aparecem quando aplicáveis:

- **user**: Quando o usuário está carregado
- **platform**: Quando a plataforma está carregada
- **platform.company**: Quando a plataforma possui empresa
- **company**: Quando a reclamação está associada a uma empresa
- **title**: Quando há títulos disponíveis
- **texts**: Quando há textos carregados
- **description**: Quando há descrição
- **images**: Quando há imagens anexadas
- **replies**: Quando há respostas
- **status_change_history**: Quando há histórico de status

## Notas

- Este endpoint retorna informações muito mais detalhadas que o index
- Todos os dados sensíveis são expostos (endpoint administrativo)
- Os emails de usuários são sempre visíveis
- O histórico de status mostra toda a evolução da reclamação
- As respostas são ordenadas por `index_number`
- O campo `updated_by` no histórico é `null` quando a alteração foi feita pelo próprio usuário da reclamação
- Textos e descrições podem conter HTML
- Todas as URLs de imagens são completas e prontas para uso
- As datas seguem o formato ISO 8601
- A empresa é retornada via `ClaimedCompanyResource` que inclui dados completos

## Relacionados

- [Listar Reclamações](BackofficeComplaintIndex.md)
- [Complaint Model](../Models/Complaint.md)
- [ClaimedCompanyResource](../Resources/ClaimedCompanyResource.md)

## Changelog

- 2025-10-26: Documentação inicial do endpoint de backoffice
