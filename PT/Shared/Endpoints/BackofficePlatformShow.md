# Shared – Exibir Detalhes da Plataforma

## Endpoint

```
GET /api/v1/backoffice/platforms/{platform}
```

## Descrição

Retorna informações detalhadas de uma plataforma específica, incluindo dados completos da empresa, configurações regionais, gateways de pagamento, imagens, configurações IMAP e estatísticas agregadas.

## Autenticação

Obrigatória – Bearer {token} com habilidade `backoffice`

## Cabeçalhos

| Cabeçalho     | Tipo | Obrigatório | Descrição |
| ---------------- | ------ | -------- | ----------- |
| Authorization    | string | Quando exigido | Credencial `Bearer {token}`. |
| X-PUBLIC-KEY     | string | Sim      | Chave pública da plataforma. |
| Accept-Language  | string | Não       | Locale IETF (ex.: `pt-BR`, `en`, `es`). |

## Parâmetros de URL

| Parâmetro | Tipo | Obrigatório | Descrição |
| ----------- | ------- | ----------- | ----------- |
| platform    | string  | Sim         | UUID da plataforma a ser exibida |

## Exemplos

### Exemplo de requisição (curl)

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: pt-BR" \
  "https://sandbox.your-domain.com/api/v1/backoffice/platforms/9d4e1c2a-3b4c-5d6e-7f8g-9h0i1j2k3l4m"
```

### Exemplo de resposta

```json
{
  "uuid": "9d4e1c2a-3b4c-5d6e-7f8g-9h0i1j2k3l4m",
  "total": {
    "users": 150
  },
  "is_parent": true,
  "domain": {
    "uuid": "8c3d0b1a-2e3f-4g5h-6i7j-8k9l0m1n2o3p",
    "slug": "ecommerce",
    "name": "E-commerce"
  },
  "name": "Plataforma Demo",
  "email": "contato@plataforma.com",
  "open_to_register": true,
  "public_key": "pk_test_123456789",
  "logo": {
    "main": "https://cdn.example.com/logos/main.png",
    "square": "https://cdn.example.com/logos/square.png",
    "rounded": "https://cdn.example.com/logos/rounded.png",
    "rectangular": "https://cdn.example.com/logos/rectangular.png"
  },
  "created_at": "2024-01-15T10:30:00Z",
  "company": {
    "uuid": "7b2c9a0b-1d2e-3f4g-5h6i-7j8k9l0m1n2o",
    "name": "Empresa Demo Ltda",
    "slug": "empresa-demo",
    "zipcode": "01310-100",
    "country": "Brasil",
    "state": "São Paulo",
    "city": "São Paulo",
    "company_logos": [
      {
        "main_logo": "https://cdn.example.com/company/main.png"
      },
      {
        "square_logo": "https://cdn.example.com/company/square.png"
      }
    ],
    "full_address": "Av. Paulista, 1000 - Bela Vista, São Paulo - SP, 01310-100",
    "links": [
      {
        "uuid": "6a1b8c9d-0e1f-2g3h-4i5j-6k7l8m9n0o1p",
        "platform": "Facebook",
        "url": "https://facebook.com/plataforma"
      },
      {
        "uuid": "5a0b7c8d-9e0f-1g2h-3i4j-5k6l7m8n9o0p",
        "platform": "Instagram",
        "url": "https://instagram.com/plataforma"
      }
    ]
  },
  "client_id": "client_abc123xyz",
  "discord": {
    "channel_id": "1234567890123456789"
  },
  "is_main": true,
  "is_banned": false,
  "platform_transactions": {
    "in": 5000,
    "out": 3000
  },
  "regional_information": {
    "uuid": "aa0e8400-e29b-41d4-a716-446655440005",
    "language": "pt-BR",
    "currency": "BRL"
  },
  "imap_setting": {
    "uuid": "bb0e8400-e29b-41d4-a716-446655440006",
    "host": "imap.gmail.com",
    "port": 993,
    "encryption": "ssl",
    "username": "contato@minhaloja.com",
    "is_active": true
  },
  "payment_gateways": [
    {
      "uuid": "cc0e8400-e29b-41d4-a716-446655440007",
      "name": "Stripe",
      "is_active": true,
      "gateway_type": "credit_card"
    },
    {
      "uuid": "dd0e8400-e29b-41d4-a716-446655440008",
      "name": "PayPal",
      "is_active": true,
      "gateway_type": "wallet"
    }
  ],
  "images": [
    {
      "uuid": "ee0e8400-e29b-41d4-a716-446655440009",
      "usage": "banner",
      "url": "https://cdn.example.com/images/banner.jpg",
      "created_at": "2024-01-20T14:30:00Z"
    },
    {
      "uuid": "ff0e8400-e29b-41d4-a716-446655440010",
      "usage": "background",
      "url": "https://cdn.example.com/images/background.jpg",
      "created_at": "2024-01-21T09:15:00Z"
    }
  ],
  "total_counts": {
    "products": 250,
    "orders": 1500,
    "articles": 45,
    "events": 12,
    "newsletters": 8
  }
}
```

## Estrutura JSON Explicada

### Campos Principais (sempre presentes)

| Campo | Tipo | Descrição |
| ----------- | ------- | ----------- |
| uuid | string  | Identificador único da plataforma |
| total | object  | Totalizadores da plataforma (condicional - apenas se houver usuários) |
| total.users | integer | Total de usuários cadastrados |
| is_parent | boolean | Indica se a plataforma é uma plataforma pai |
| domain | object  | Informações da área de domínio |
| domain.uuid | string  | Identificador único da área de domínio |
| domain.slug | string  | Slug da área de domínio |
| domain.name | string  | Nome da área de domínio |
| name | string  | Nome da plataforma |
| email | string  | Email de contato da plataforma |
| open_to_register | boolean | Indica se está aberta para novos registros |
| public_key | string  | Chave pública da plataforma |
| logo | object  | URLs dos logos da plataforma |
| logo.main | string  | URL do logo principal |
| logo.square | string  | URL do logo quadrado |
| logo.rounded | string  | URL do logo arredondado |
| logo.rectangular | string  | URL do logo retangular |
| created_at | string  | Data de criação (ISO 8601) |

### Campos da Empresa (condicional)

Estes campos aparecem dentro do objeto `company` apenas quando a plataforma possui uma empresa associada:

| Campo | Tipo | Descrição |
| ----------- | ------- | ----------- |
| company | object  | Dados da empresa (condicional) |
| company.uuid | string  | UUID da empresa |
| company.name | string  | Nome da empresa |
| company.slug | string  | Slug da empresa |
| company.zipcode | string\|null | CEP |
| company.country | string\|null | País |
| company.state | string\|null | Estado |
| company.city | string\|null | Cidade |
| company.company_logos | array  | Logos da empresa |
| company.full_address | string\|null | Endereço completo formatado |
| company.links | array  | Redes sociais |
| company.links[].uuid | string  | UUID do link social |
| company.links[].platform | string  | Nome da plataforma social |
| company.links[].url | string  | URL do perfil |

### Campos Detalhados (exclusivos do show)

| Campo | Tipo | Descrição |
| ----------- | ------- | ----------- |
| client_id | string  | ID do cliente OAuth |
| discord | object  | Configurações do Discord |
| discord.channel_id | string  | ID do canal Discord para notificações |

### Campos de Afiliação (condicional - platformAffiliated)

Aparecem apenas quando a plataforma possui relação de afiliação:

| Campo | Tipo | Descrição |
| ----------- | ------- | ----------- |
| is_main | boolean | Se é plataforma principal |
| is_banned | boolean | Se está banida |
| platform_transactions | object  | Transações da plataforma |
| platform_transactions.in | integer | Transações de entrada |
| platform_transactions.out | integer | Transações de saída |

### Informações Regionais (condicional)

| Campo | Tipo | Descrição |
| ----------- | ------- | ----------- |
| regional_information | object  | Informações regionais (condicional) |
| regional_information.uuid | string  | UUID das informações regionais |
| regional_information.language | string  | Código do idioma (ex: pt-BR, en, es) |
| regional_information.currency | string  | Código da moeda (ex: BRL, USD, EUR) |

### Configurações IMAP (condicional)

| Campo | Tipo | Descrição |
| ----------- | ------- | ----------- |
| imap_setting | object  | Configurações IMAP (condicional) |
| imap_setting.uuid | string  | UUID da configuração |
| imap_setting.host | string  | Host do servidor IMAP |
| imap_setting.port | integer | Porta do servidor IMAP |
| imap_setting.encryption | string  | Tipo de criptografia (ssl, tls, none) |
| imap_setting.username | string  | Usuário de autenticação |
| imap_setting.is_active | boolean | Se a configuração está ativa |

### Gateways de Pagamento (condicional)

| Campo | Tipo | Descrição |
| ----------- | ------- | ----------- |
| payment_gateways | array  | Lista de gateways (condicional) |
| payment_gateways[].uuid | string  | UUID do gateway |
| payment_gateways[].name | string  | Nome do gateway |
| payment_gateways[].is_active | boolean | Se está ativo |
| payment_gateways[].gateway_type | string  | Tipo do gateway |

### Imagens (condicional)

| Campo | Tipo | Descrição |
| ----------- | ------- | ----------- |
| images | array  | Imagens da plataforma (condicional) |
| images[].uuid | string  | UUID da imagem |
| images[].usage | string  | Uso da imagem (banner, background, etc) |
| images[].url | string  | URL completa da imagem |
| images[].created_at | string  | Data de criação (ISO 8601) |

### Contadores Agregados

| Campo | Tipo | Descrição |
| ----------- | ------- | ----------- |
| total_counts | object  | Contadores agregados |
| total_counts.products | integer | Total de produtos (apenas se > 0) |
| total_counts.orders | integer | Total de pedidos (apenas se > 0) |
| total_counts.articles | integer | Total de artigos (apenas se > 0) |
| total_counts.events | integer | Total de eventos (apenas se > 0) |
| total_counts.newsletters | integer | Total de newsletters (apenas se > 0) |

## Relações Carregadas

Este endpoint carrega automaticamente as seguintes relações:

- `domainArea` - Área de domínio
- `company` - Empresa associada
- `company.logos` - Logos da empresa
- `company.address` - Endereço da empresa
- `company.socialMedias` - Redes sociais da empresa
- `regionalInformation` - Informações regionais
- `imapSetting` - Configurações IMAP
- `userRoles` - Papéis de usuários
- `userRoles.user` - Dados dos usuários
- `paymentGateways` - Gateways de pagamento
- `images` - Imagens da plataforma
- `affiliated` - Plataformas afiliadas

## Contadores

- `users_count` - Total de usuários
- `products_count` - Total de produtos
- `orders_count` - Total de pedidos
- `articles_count` - Total de artigos
- `events_count` - Total de eventos
- `newsletters_count` - Total de newsletters

## Status HTTP

- 200: OK
- 401: Não autenticado
- 403: Proibido (sem permissão `show.all`)
- 404: Plataforma não encontrada
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

- **company**: Apenas quando a plataforma possui empresa associada
- **is_main, is_banned, platform_transactions**: Apenas quando possui relação platformAffiliated
- **regional_information**: Apenas quando a relação está carregada
- **imap_setting**: Apenas quando configurado e a relação está carregada
- **payment_gateways**: Apenas quando a relação está carregada
- **images**: Apenas quando a relação está carregada
- **Contadores em total_counts**: Apenas se o valor for maior que 0

## Notas

- Este endpoint retorna informações muito mais detalhadas que o index
- O `client_id` é exposto apenas no show, não no index
- As configurações do Discord incluem o canal específico da plataforma ou o canal padrão
- Todos os arrays retornam vazio quando as relações não estão disponíveis
- Todas as datas seguem o formato ISO 8601
- URLs de logo e imagens são completas e prontas para uso
- Os contadores agregados fornecem visão geral da atividade da plataforma

## Relacionados

- [Listar Plataformas](BackofficePlatformIndex.md)
- [Listar Usuários da Plataforma](BackofficePlatformUserIndex.md)

## Changelog

- 2025-10-25: Atualização completa com todos os campos detalhados incluindo client_id, discord, regional_information, imap_setting, payment_gateways, images e total_counts
- 2025-10-16: Documentação inicial
