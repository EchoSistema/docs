# Shared – Listar Plataformas

## Endpoint

```
GET /api/v1/backoffice/platforms
```

## Autenticação

Obrigatória – Bearer {token} com habilidade `backoffice`

## Cabeçalhos

| Cabeçalho     | Tipo | Obrigatório | Descrição |
| ---------------- | ------ | -------- | ----------- |
| Authorization    | string | Quando exigido | Credencial `Bearer {token}`. |
| X-PUBLIC-KEY     | string | Sim      | Chave pública da plataforma. |
| Accept-Language  | string | Não       | Locale IETF (ex.: `pt-BR`, `en`, `es`). |

## Parâmetros de Query

| Parâmetro | Tipo | Obrigatório | Descrição |
| ----------- | ------- | ----------- | ----------- |
| no_paginate | boolean | Não         | Se `true`, retorna todos os resultados sem paginação. |
| per_page    | integer | Não         | Número de registros por página (padrão: 25). |

## Exemplos

### Exemplo de requisição (curl)

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: pt-BR" \
  "https://sandbox.your-domain.com/api/v1/backoffice/platforms?per_page=25"
```

### Exemplo de resposta

```json
{
  "data": [
    {
      "uuid": "9d4e1c2a-3b4c-5d6e-7f8g-9h0i1j2k3l4m",
      "total": {
        "users": 150
      },
      "domain": {
        "uuid": "8c3d0b1a-2e3f-4g5h-6i7j-8k9l0m1n2o3p",
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
      "company_uuid": "7b2c9a0b-1d2e-3f4g-5h6i-7j8k9l0m1n2o",
      "slug": "plataforma-demo",
      "country": "Brasil",
      "state": "São Paulo",
      "city": "São Paulo",
      "company_logos": [
        {
          "main": "https://cdn.example.com/company/main.png"
        },
        {
          "square": "https://cdn.example.com/company/square.png"
        }
      ],
      "full_address": "Av. Paulista, 1000 - Bela Vista, São Paulo - SP, 01310-100",
      "links": [
        {
          "uuid": "6a1b8c9d-0e1f-2g3h-4i5j-6k7l8m9n0o1p",
          "platform": "facebook",
          "url": "https://facebook.com/plataforma"
        },
        {
          "uuid": "5a0b7c8d-9e0f-1g2h-3i4j-5k6l7m8n9o0p",
          "platform": "instagram",
          "url": "https://instagram.com/plataforma"
        }
      ]
    }
  ],
  "links": {
    "first": "https://sandbox.your-domain.com/api/v1/backoffice/platforms?page=1",
    "last": "https://sandbox.your-domain.com/api/v1/backoffice/platforms?page=5",
    "prev": null,
    "next": "https://sandbox.your-domain.com/api/v1/backoffice/platforms?page=2"
  },
  "meta": {
    "current_page": 1,
    "from": 1,
    "last_page": 5,
    "per_page": 25,
    "to": 25,
    "total": 125
  }
}
```

## Estrutura JSON Explicada

| Campo | Tipo | Descrição |
| ----------- | ------- | ----------- |
| data        | array   | Lista de plataformas |
| data[].uuid | string  | Identificador único da plataforma |
| data[].total | object  | Totalizadores da plataforma |
| data[].total.users | integer | Total de usuários na plataforma |
| data[].domain | object  | Informações da área de domínio |
| data[].domain.uuid | string  | Identificador único da área de domínio |
| data[].domain.name | string  | Nome da área de domínio |
| data[].name | string  | Nome da plataforma |
| data[].email | string  | Email de contato da plataforma |
| data[].open_to_register | boolean | Indica se a plataforma está aberta para novos registros |
| data[].public_key | string  | Chave pública da plataforma |
| data[].logo | object  | URLs dos logos da plataforma |
| data[].logo.main | string  | URL do logo principal |
| data[].logo.square | string  | URL do logo quadrado |
| data[].logo.rounded | string  | URL do logo arredondado |
| data[].logo.rectangular | string  | URL do logo retangular |
| data[].created_at | string  | Data de criação (ISO 8601) |
| data[].company_uuid | string  | Identificador único da empresa (quando disponível) |
| data[].slug | string  | Slug da empresa (quando disponível) |
| data[].country | string  | País da empresa (quando disponível) |
| data[].state | string  | Estado da empresa (quando disponível) |
| data[].city | string  | Cidade da empresa (quando disponível) |
| data[].company_logos | array   | Logos da empresa (quando disponível) |
| data[].full_address | string  | Endereço completo formatado (quando disponível) |
| data[].links | array   | Links de redes sociais da empresa (quando disponível) |
| data[].links[].uuid | string  | Identificador único do link |
| data[].links[].platform | string  | Nome da plataforma social |
| data[].links[].url | string  | URL do perfil social |
| links       | object  | Links de paginação |
| meta        | object  | Metadados da paginação |

## Status HTTP

- 200: OK
- 201: Criado
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
  "message": "Mensagem de erro"
}
```

## Notas

- Este endpoint retorna todas as plataformas cadastradas no sistema
- Apenas usuários com permissão de backoffice podem acessar
- A resposta inclui relacionamentos com company, company.logos e company.address
- Inclui contagem de usuários por plataforma (users_count)
- Ordenado por data de criação (decrescente)

## Relacionados

- [Exibir Detalhes da Plataforma](BackofficePlatformShow.md)
- [Listar Usuários da Plataforma](BackofficePlatformUserIndex.md)

## Changelog

- 2025-10-25: Atualização completa da documentação com estrutura detalhada
- 2025-10-16: Documentação inicial
