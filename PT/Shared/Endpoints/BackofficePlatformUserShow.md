# Shared – Exibir Detalhes do Usuário

## Endpoint

```
GET /api/v1/backoffice/users/{user}
```

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
| user        | string  | Sim         | UUID, Echo UUID ou ID do usuário a ser exibido |

## Exemplos

### Exemplo de requisição (curl)

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: pt-BR" \
  "https://sandbox.your-domain.com/api/v1/backoffice/users/9d4e1c2a-3b4c-5d6e-7f8g-9h0i1j2k3l4m"
```

### Exemplo de resposta

```json
{
  "data": {
    "id": 1234,
    "echo_uuid": "echo_9d4e1c2a3b4c5d6e",
    "uuid": "9d4e1c2a-3b4c-5d6e-7f8g-9h0i1j2k3l4m",
    "name": "João Silva",
    "gender": {
      "symbol": "M",
      "name": "Masculino"
    },
    "age": 32,
    "birth_date": "1992-05-15T00:00:00Z",
    "email": "joao.silva@example.com",
    "avatar": "https://cdn.example.com/avatars/joao-silva.jpg",
    "created_at": "2024-01-15T10:30:00Z",
    "roles": [
      {
        "id": 5,
        "main": true,
        "platform": "E-commerce Platform",
        "platform_uuid": "8c3d0b1a-2e3f-4g5h-6i7j-8k9l0m1n2o3p",
        "domain": "E-commerce",
        "role": "Admin",
        "language": "pt-BR",
        "currency": "BRL",
        "status": "active",
        "created_at": "2024-01-15T10:30:00Z"
      },
      {
        "id": 7,
        "main": false,
        "platform": "Articles Platform",
        "platform_uuid": "7b2c9a0b-1d2e-3f4g-5h6i-7j8k9l0m1n2o",
        "domain": "Articles",
        "role": "Editor",
        "language": "pt-BR",
        "currency": "BRL",
        "status": "active",
        "created_at": "2024-02-20T14:15:00Z"
      }
    ],
    "updated_at": "2024-10-15T08:20:00Z",
    "language": "pt-BR",
    "currency": "BRL",
    "telephone": "+5511999887766",
    "slug": "joao-silva",
    "is_banned": false,
    "is_foreign": false,
    "is_master": false,
    "email_verified_at": "2024-01-15T11:00:00Z",
    "platform": {
      "uuid": "8c3d0b1a-2e3f-4g5h-6i7j-8k9l0m1n2o3p",
      "name": "E-commerce Platform",
      "domain_area": "E-commerce"
    },
    "contacts": [
      {
        "uuid": "6a1b8c9d-0e1f-2g3h-4i5j-6k7l8m9n0o1p",
        "type": "mobile",
        "country_code": "+55",
        "number": "11999887766",
        "phone": "+5511999887766",
        "email": null,
        "created_at": "2024-01-15T10:30:00Z"
      },
      {
        "uuid": "5a0b7c8d-9e0f-1g2h-3i4j-5k6l7m8n9o0p",
        "type": "email",
        "country_code": null,
        "number": null,
        "phone": null,
        "email": "joao.silva.alt@example.com",
        "created_at": "2024-02-10T14:20:00Z"
      }
    ],
    "social_medias": [
      {
        "uuid": "4a9b6c7d-8e9f-0g1h-2i3j-4k5l6m7n8o9p",
        "name": "LinkedIn",
        "url": "https://linkedin.com/in/joaosilva",
        "created_at": "2024-01-15T10:30:00Z"
      },
      {
        "uuid": "3a8b5c6d-7e8f-9g0h-1i2j-3k4l5m6n7o8p",
        "name": "Twitter",
        "url": "https://twitter.com/joaosilva",
        "created_at": "2024-01-15T10:30:00Z"
      }
    ],
    "address": {
      "uuid": "2a7b4c5d-6e7f-8g9h-0i1j-2k3l4m5n6o7p",
      "zipcode": "01310-100",
      "street": "Avenida Paulista",
      "number": "1000",
      "complement": "Apto 501",
      "neighborhood": "Bela Vista",
      "city": "São Paulo",
      "state": "São Paulo",
      "country": "Brasil",
      "formatted": "Avenida Paulista, 1000, Apto 501 - Bela Vista, São Paulo - SP, Brasil, 01310-100"
    },
    "nationalities": [
      {
        "uuid": "1a6b3c4d-5e6f-7g8h-9i0j-1k2l3m4n5o6p",
        "country": "Brasil"
      }
    ],
    "biography": {
      "uuid": "0a5b2c3d-4e5f-6g7h-8i9j-0k1l2m3n4o5p",
      "summary": "Profissional experiente em gestão de e-commerce com mais de 10 anos de experiência."
    },
    "job_occupation": {
      "uuid": "9a4b1c2d-3e4f-5g6h-7i8j-9k0l1m2n3o4p",
      "occupation": "Gerente de E-commerce",
      "company": "Tech Solutions Ltda",
      "is_default": true
    },
    "job_occupations": [
      {
        "uuid": "9a4b1c2d-3e4f-5g6h-7i8j-9k0l1m2n3o4p",
        "occupation": "Gerente de E-commerce",
        "company": "Tech Solutions Ltda",
        "is_default": true,
        "started_at": "2020-01-15T00:00:00Z",
        "ended_at": null
      },
      {
        "uuid": "8a3b0c1d-2e3f-4g5h-6i7j-8k9l0m1n2o3p",
        "occupation": "Analista de Marketing",
        "company": "Marketing Corp",
        "is_default": false,
        "started_at": "2017-03-01T00:00:00Z",
        "ended_at": "2019-12-31T00:00:00Z"
      }
    ],
    "affiliate": {
      "uuid": "7a2b9c0d-1e2f-3g4h-5i6j-7k8l9m0n1o2p",
      "token": "aff_tkn_123456789abcdef",
      "created_at": "2024-01-15T10:30:00Z"
    },
    "identities": [
      {
        "uuid": "6a1b8c9d-0e1f-2g3h-4i5j-6k7l8m9n0o1p",
        "type": "cpf",
        "number": "123.456.789-00",
        "verified_at": "2024-01-15T11:00:00Z"
      }
    ]
  }
}
```

## Estrutura JSON Explicada

### Campos Base (sempre presentes)

| Campo | Tipo | Descrição |
| ----------- | ------- | ----------- |
| data        | object  | Dados do usuário |
| data.id     | integer | ID interno do usuário |
| data.echo_uuid | string  | UUID do EchoSistema (formato: echo_*) |
| data.uuid   | string  | Identificador único do usuário |
| data.name   | string  | Nome completo do usuário |
| data.gender | object  | Informações de gênero |
| data.gender.symbol | string  | Símbolo do gênero (M, F, O) |
| data.gender.name | string  | Nome do gênero por extenso |
| data.age    | integer | Idade calculada do usuário |
| data.birth_date | string  | Data de nascimento (ISO 8601) |
| data.email  | string  | Email do usuário |
| data.avatar | string  | URL da imagem de avatar |
| data.created_at | string  | Data de criação da conta (ISO 8601) |
| data.roles  | array   | Lista de funções/papéis do usuário em diferentes plataformas |

### Campos Adicionais (modo detalhado)

| Campo | Tipo | Descrição |
| ----------- | ------- | ----------- |
| data.updated_at | string  | Data da última atualização (ISO 8601) |
| data.language | string  | Idioma preferencial do usuário (IETF locale) |
| data.currency | string  | Moeda preferencial (código ISO) |
| data.telephone | string  | Número de telefone principal formatado |
| data.slug   | string  | Slug único do usuário |
| data.is_banned | boolean | Indica se o usuário está banido |
| data.is_foreign | boolean | Indica se o usuário é estrangeiro |
| data.is_master | boolean | Indica se o usuário possui privilégios master |
| data.email_verified_at | string  | Data de verificação do email (ISO 8601) |

### Plataforma (quando disponível)

| Campo | Tipo | Descrição |
| ----------- | ------- | ----------- |
| data.platform | object  | Dados da plataforma principal |
| data.platform.uuid | string  | UUID da plataforma |
| data.platform.name | string  | Nome da plataforma |
| data.platform.domain_area | string  | Área de domínio da plataforma |

### Contatos (quando disponível)

| Campo | Tipo | Descrição |
| ----------- | ------- | ----------- |
| data.contacts | array   | Lista de contatos do usuário |
| data.contacts[].uuid | string  | UUID do contato |
| data.contacts[].type | string  | Tipo de contato (mobile, email, phone, etc.) |
| data.contacts[].country_code | string  | Código do país (ex: +55) |
| data.contacts[].number | string  | Número sem código do país |
| data.contacts[].phone | string  | Telefone completo formatado |
| data.contacts[].email | string  | Email (quando tipo é email) |
| data.contacts[].created_at | string  | Data de criação (ISO 8601) |

### Redes Sociais (quando disponível)

| Campo | Tipo | Descrição |
| ----------- | ------- | ----------- |
| data.social_medias | array   | Lista de redes sociais |
| data.social_medias[].uuid | string  | UUID da rede social |
| data.social_medias[].name | string  | Nome da rede social |
| data.social_medias[].url | string  | URL do perfil |
| data.social_medias[].created_at | string  | Data de criação (ISO 8601) |

### Endereço (quando disponível)

| Campo | Tipo | Descrição |
| ----------- | ------- | ----------- |
| data.address | object  | Endereço do usuário |
| data.address.uuid | string  | UUID do endereço |
| data.address.zipcode | string  | CEP/Código postal |
| data.address.street | string  | Nome da rua/avenida |
| data.address.number | string  | Número |
| data.address.complement | string  | Complemento |
| data.address.neighborhood | string  | Bairro |
| data.address.city | string  | Cidade |
| data.address.state | string  | Estado/Província |
| data.address.country | string  | País |
| data.address.formatted | string  | Endereço completo formatado |

### Nacionalidades (quando disponível)

| Campo | Tipo | Descrição |
| ----------- | ------- | ----------- |
| data.nationalities | array   | Lista de nacionalidades |
| data.nationalities[].uuid | string  | UUID da nacionalidade |
| data.nationalities[].country | string  | Nome do país |

### Informações de Banimento (quando disponível)

| Campo | Tipo | Descrição |
| ----------- | ------- | ----------- |
| data.banned_info | object  | Informações sobre o banimento (apenas se usuário estiver banido) |
| data.banned_info.reason | string  | Motivo do banimento |
| data.banned_info.banned_at | string  | Data do banimento (ISO 8601) |
| data.banned_info.until_date | string  | Data final do banimento (ISO 8601), null se permanente |

### Biografia (quando disponível)

| Campo | Tipo | Descrição |
| ----------- | ------- | ----------- |
| data.biography | object  | Biografia do usuário |
| data.biography.uuid | string  | UUID da biografia |
| data.biography.summary | string  | Texto da biografia |

### Ocupação Profissional (quando disponível)

| Campo | Tipo | Descrição |
| ----------- | ------- | ----------- |
| data.job_occupation | object  | Ocupação profissional padrão atual |
| data.job_occupation.uuid | string  | UUID da ocupação |
| data.job_occupation.occupation | string  | Nome da ocupação |
| data.job_occupation.company | string  | Nome da empresa |
| data.job_occupation.is_default | boolean | Indica se é a ocupação padrão |

### Histórico de Ocupações (quando disponível)

| Campo | Tipo | Descrição |
| ----------- | ------- | ----------- |
| data.job_occupations | array   | Histórico completo de ocupações |
| data.job_occupations[].uuid | string  | UUID da ocupação |
| data.job_occupations[].occupation | string  | Nome da ocupação |
| data.job_occupations[].company | string  | Nome da empresa |
| data.job_occupations[].is_default | boolean | Indica se é a ocupação padrão |
| data.job_occupations[].started_at | string  | Data de início (ISO 8601) |
| data.job_occupations[].ended_at | string  | Data de término (ISO 8601), null se atual |

### Afiliado (quando disponível)

| Campo | Tipo | Descrição |
| ----------- | ------- | ----------- |
| data.affiliate | object  | Informações de afiliado (apenas se usuário for afiliado) |
| data.affiliate.uuid | string  | UUID do afiliado |
| data.affiliate.token | string  | Token de referência do afiliado |
| data.affiliate.created_at | string  | Data de registro como afiliado (ISO 8601) |

### Documentos de Identidade (quando disponível)

| Campo | Tipo | Descrição |
| ----------- | ------- | ----------- |
| data.identities | array   | Lista de documentos de identidade |
| data.identities[].uuid | string  | UUID do documento |
| data.identities[].type | string  | Tipo de documento (cpf, rg, passport, etc.) |
| data.identities[].number | string  | Número do documento |
| data.identities[].verified_at | string  | Data de verificação (ISO 8601) |

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

- Este endpoint retorna todos os detalhes de um usuário específico
- Apenas usuários com permissão `show.all` de backoffice podem acessar
- O parâmetro `{user}` aceita UUID, Echo UUID ou ID do usuário
- A resposta inclui os seguintes relacionamentos carregados:
  - `regionalInformation`: Informações regionais (idioma, moeda)
  - `identities`: Documentos de identidade
  - `telephone`: Telefone principal
  - `avatarImage`: Imagem do avatar
  - `profileImage`: Imagem do perfil
  - `platform`: Plataforma principal
  - `platform.domainArea`: Área de domínio da plataforma principal
  - `platformRoles`: Todas as funções em plataformas
  - `platformRoles.platform`: Plataformas das funções
  - `platformRoles.platform.domainArea`: Áreas de domínio das plataformas
  - `platformRoles.role`: Detalhes das funções
  - `contacts`: Contatos do usuário
  - `socialMedias`: Redes sociais
  - `address`: Endereço completo com cidade, estado e país
  - `nationalities`: Nacionalidades
  - `banned`: Informações de banimento (se aplicável)
  - `biography`: Biografia
  - `jobOccupation`: Ocupação profissional atual padrão
  - `jobOccupations`: Histórico completo de ocupações
  - `affiliate`: Dados de afiliado (se aplicável)
- Campos condicionais aparecem apenas quando os relacionamentos estão carregados e possuem dados
- O campo `avatar` é gerado dinamicamente através do método `getAvatar()`
- O campo `age` é calculado automaticamente a partir da data de nascimento

## Relacionados

- [Listar Todos os Usuários](BackofficePlatformUserIndex.md)
- [Listar Plataformas](BackofficePlatformIndex.md)

## Changelog

- 2025-10-25: Documentação completa com todos os campos e relacionamentos detalhados
- 2025-10-16: Documentação inicial
