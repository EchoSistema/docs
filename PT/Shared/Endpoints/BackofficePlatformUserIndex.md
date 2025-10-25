# Shared – Listar Todos os Usuários

## Endpoint

```
GET /api/v1/backoffice/users
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
  "https://sandbox.your-domain.com/api/v1/backoffice/users?per_page=25"
```

### Exemplo de resposta

```json
{
  "data": [
    {
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
      "currency": "BRL",
      "language": "pt-BR",
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
      ]
    },
    {
      "id": 1235,
      "echo_uuid": "echo_8c3d0b1a2e3f4g5h",
      "uuid": "8c3d0b1a-2e3f-4g5h-6i7j-8k9l0m1n2o3p",
      "name": "Maria Santos",
      "gender": {
        "symbol": "F",
        "name": "Feminino"
      },
      "age": 28,
      "birth_date": "1996-08-22T00:00:00Z",
      "email": "maria.santos@example.com",
      "avatar": "https://cdn.example.com/avatars/maria-santos.jpg",
      "currency": "BRL",
      "language": "pt-BR",
      "created_at": "2024-01-20T15:45:00Z",
      "roles": [
        {
          "id": 3,
          "main": true,
          "platform": "RealEstate Platform",
          "platform_uuid": "6a1b8c9d-0e1f-2g3h-4i5j-6k7l8m9n0o1p",
          "domain": "RealEstate",
          "role": "Agent",
          "language": "pt-BR",
          "currency": "BRL",
          "status": "active",
          "created_at": "2024-01-20T15:45:00Z"
        }
      ]
    }
  ],
  "links": {
    "first": "https://sandbox.your-domain.com/api/v1/backoffice/users?page=1",
    "last": "https://sandbox.your-domain.com/api/v1/backoffice/users?page=10",
    "prev": null,
    "next": "https://sandbox.your-domain.com/api/v1/backoffice/users?page=2"
  },
  "meta": {
    "current_page": 1,
    "from": 1,
    "last_page": 10,
    "per_page": 25,
    "to": 25,
    "total": 250
  }
}
```

## Estrutura JSON Explicada

| Campo | Tipo | Descrição |
| ----------- | ------- | ----------- |
| data        | array   | Lista de usuários |
| data[].id   | integer | ID interno do usuário |
| data[].echo_uuid | string  | UUID do EchoSistema (formato: echo_*) |
| data[].uuid | string  | Identificador único do usuário |
| data[].name | string  | Nome completo do usuário |
| data[].gender | object  | Informações de gênero |
| data[].gender.symbol | string  | Símbolo do gênero (M, F, O) |
| data[].gender.name | string  | Nome do gênero por extenso |
| data[].age  | integer | Idade calculada do usuário |
| data[].birth_date | string  | Data de nascimento (ISO 8601) |
| data[].email | string  | Email do usuário |
| data[].avatar | string  | URL da imagem de avatar |
| data[].currency | string  | Moeda preferencial do usuário (código ISO) |
| data[].language | string  | Idioma preferencial do usuário (IETF locale) |
| data[].created_at | string  | Data de criação da conta (ISO 8601) |
| data[].roles | array   | Lista de funções/papéis do usuário em diferentes plataformas |
| data[].roles[].id | integer | ID da função |
| data[].roles[].main | boolean | Indica se é a plataforma principal do usuário |
| data[].roles[].platform | string  | Nome da plataforma |
| data[].roles[].platform_uuid | string  | UUID da plataforma |
| data[].roles[].domain | string  | Área de domínio da plataforma |
| data[].roles[].role | string  | Nome da função/papel |
| data[].roles[].language | string  | Idioma configurado (IETF locale) |
| data[].roles[].currency | string  | Moeda configurada (código ISO) |
| data[].roles[].status | string  | Status da função (active, inactive, etc.) |
| data[].roles[].created_at | string  | Data de atribuição da função (ISO 8601) |
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

- Este endpoint retorna todos os usuários de todas as plataformas do sistema
- Apenas usuários com permissão `index.all` de backoffice podem acessar
- A resposta inclui os seguintes relacionamentos carregados:
  - `regionalInformation`: Informações regionais do usuário (idioma, moeda)
  - `identities`: Documentos de identidade do usuário
  - `telephone`: Telefone principal
  - `avatarImage`: Imagem do avatar
  - `profileImage`: Imagem do perfil
  - `platform`: Plataforma principal do usuário
  - `platformRoles`: Todas as funções do usuário em diferentes plataformas
  - `platformRoles.platform`: Dados das plataformas associadas às funções
- O campo `avatar` é gerado dinamicamente através do método `getAvatar()`
- O campo `age` é calculado automaticamente a partir da data de nascimento
- Os campos `currency` e `language` são acessores que retornam dados de `regionalInformation`
- O campo `roles` consolida todas as funções do usuário com indicação da plataforma principal
- Ordenado por padrão pela data de criação

## Relacionados

- [Exibir Detalhes do Usuário](BackofficePlatformUserShow.md)
- [Listar Plataformas](BackofficePlatformIndex.md)

## Changelog

- 2025-10-25: Atualização completa da documentação com estrutura detalhada e todos os campos
- 2025-10-16: Documentação inicial
