# Shared — Listar Usuários das Plataformas (Backoffice)

## Endpoint

```
GET /api/v1/backoffice/users
```

## Autenticação

Obrigatória – Bearer {token} com permissão `index.all`

## Cabeçalhos

| Cabeçalho          | Tipo     | Obrigatório | Descrição |
| ------------------ | -------- | ----------- | --------- |
| Authorization      | string   | Sim         | Credencial `Bearer {token}`. |
| X-PUBLIC-KEY       | string   | Sim         | Chave pública da plataforma. |
| Accept-Language    | string   | Não         | Locale IETF (ex.: `pt-BR`, `en`, `es`). |

## Parâmetros

### Parâmetros de consulta

| Parâmetro     | Tipo    | Obrigatório | Descrição | Padrão/Valores |
| ------------- | ------- | ----------- | --------- | -------------- |
| per_page      | integer | Não         | Número de itens por página. | `25` |
| page          | integer | Não         | Página atual (quando paginado). | `1` |
| no_paginate   | boolean | Não         | Se `true`, retorna todos os registros sem paginação. | `false` |

> Os parâmetros aceitam variantes em `snake_case`, `camelCase` e `kebab-case`.

## Exemplos

### Exemplo de requisição (curl)

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <chave>" \
  -H "Accept-Language: pt-BR" \
  "https://sandbox.seu-dominio.com/api/v1/backoffice/users?per_page=25&page=1"
```

### Exemplo de resposta (200 - Paginado)

```json
{
  "data": [
    {
      "id": 1234,
      "echo_uuid": "echo-550e8400-e29b-41d4-a716",
      "name": "Maria Silva",
      "gender": {
        "symbol": "F",
        "name": "Feminino"
      },
      "age": 32,
      "birth_date": "1992-05-15T00:00:00+00:00",
      "email": "maria.silva@example.com",
      "avatar": "https://cdn.example.com/avatars/maria.webp",
      "created_at": "2024-01-15T10:30:00+00:00",
      "roles": [
        {
          "id": 2,
          "main": true,
          "platform": "Echo Educação",
          "platform_uuid": "75f508e7-83ba-451c-9c2a-3df2aaf9db11",
          "domain": "Educação",
          "role": "Admin",
          "language": "pt-BR",
          "currency": "BRL",
          "status": "active",
          "created_at": "2024-01-15T10:35:00+00:00"
        }
      ]
    }
  ],
  "links": {
    "first": "https://api.example.com/api/v1/backoffice/users?page=1",
    "last": "https://api.example.com/api/v1/backoffice/users?page=10",
    "prev": null,
    "next": "https://api.example.com/api/v1/backoffice/users?page=2"
  },
  "meta": {
    "current_page": 1,
    "from": 1,
    "last_page": 10,
    "path": "https://api.example.com/api/v1/backoffice/users",
    "per_page": 25,
    "to": 25,
    "total": 250
  }
}
```

### Exemplo de resposta (200 - Sem paginação)

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <chave>" \
  "https://sandbox.seu-dominio.com/api/v1/backoffice/users?no_paginate=true"
```

```json
{
    "data": [
        {
            "id": 1234,
            "echo_uuid": "echo-550e8400-e29b-41d4-a716",
            "name": "Maria Silva",
            "gender": {
                "symbol": "F",
                "name": "Feminino"
            },
            "age": 32,
            "birth_date": "1992-05-15T00:00:00+00:00",
            "email": "maria.silva@example.com",
            "avatar": {
                "usage": "avatar",
                "url": "https://cdn.example.com/avatars/maria.webp"
            },
            "created_at": "2024-01-15T10:30:00+00:00",
            "roles": [
                {
                    "id": 1,
                    "main": true,
                    "platform": "EchoSistema",
                    "platform_uuid": "75f508e7-83ba-451c-9c2a-3df2aaf9db11",
                    "domain": "Sistema",
                    "role": "Admin",
                    "language": "pt-BR",
                    "currency": "BRL",
                    "status": "approved",
                    "created_at": "2024-01-15T10:35:00+00:00"
                }
            ]
        }
    ]
}
```

## Estrutura JSON Explicada

| Campo                   | Tipo     | Descrição |
| ----------------------- | -------- | --------- |
| data[]                  | array    | Lista de usuários |
| data[].id               | integer  | ID interno do usuário |
| data[].echo_uuid        | string   | Identificador único Echo do usuário |
| data[].name             | string   | Nome completo do usuário |
| data[].gender           | object   | Objeto com informações de gênero |
| data[].gender.symbol    | string   | Símbolo do gênero (ex: `M`, `F`) |
| data[].gender.name      | string   | Nome do gênero traduzido |
| data[].age              | integer  | Idade calculada do usuário |
| data[].birth_date       | string   | Data de nascimento (ISO 8601) |
| data[].email            | string   | E-mail do usuário |
| data[].avatar           | string   | URL do avatar do usuário |
| data[].created_at       | string   | Data de criação (ISO 8601) |
| data[].roles[]          | array    | Lista de papéis do usuário em plataformas |
| data[].roles[].id       | integer  | ID do papel/role |
| data[].roles[].main     | boolean  | Se é a plataforma principal do usuário |
| data[].roles[].platform | string   | Nome da plataforma |
| data[].roles[].platform_uuid | string | UUID da plataforma |
| data[].roles[].domain   | string   | Área de domínio da plataforma |
| data[].roles[].role     | string   | Nome do papel/cargo |
| data[].roles[].language | string   | Idioma da plataforma (ex: `pt-BR`) |
| data[].roles[].currency | string   | Moeda da plataforma (ex: `BRL`, `USD`) |
| data[].roles[].status   | string   | Status do papel (ex: `active`, `inactive`) |
| data[].roles[].created_at | string | Data de criação do papel (ISO 8601) |
| links                   | object   | Links de paginação (quando paginado) |
| meta                    | object   | Metadados de paginação (quando paginado) |

**Nota sobre typo:** Existe um typo no código (`staus` ao invés de `status`) em `BackofficeUserResource.php:38`. O campo retornado é `staus`.

## Status HTTP

- 200: Sucesso
- 401: Não autenticado
- 403: Proibido (sem permissão `index.all`)
- 429: Limite de requisições excedido
- 500: Erro interno

## Erros

### 403 Forbidden
Usuário não possui a permissão necessária:

```json
{
  "message": "Forbidden"
}
```

### 401 Unauthorized
Token ausente ou inválido:

```json
{
  "message": "Unauthenticated."
}
```

## Notas

- Este endpoint retorna usuários com eager loading de: `regionalInformation`, `identities`, `telephone`, `avatarImage`, `profileImage`, `platform`, `platformRoles`, `platformRoles.platform`
- A resposta usa `BackofficeUserCollection` e `BackofficeUserResource` para formatação
- Quando `is_index=true` (padrão neste endpoint), campos extras como `updated_at`, `language`, `currency` e `telephone` do usuário **não** são incluídos na resposta (ver `BackofficeUserResource.php:41-48`)
- O parâmetro `no_paginate=true` deve ser usado com cuidado em ambientes com muitos usuários
- O campo `main` em `roles[]` indica se aquela plataforma é a plataforma principal do usuário
- Existe um typo no Resource: o campo retornado é `staus` ao invés de `status` (linha 38 do BackofficeUserResource.php)

## Relacionados

- [Listar Plataformas](./BackofficePlatformIndex.md)
- [Listar Áreas de Domínio](./BackofficeDomainAreaIndex.md)
- [Listar Roles](./RoleIndex.md)

## Changelog

- 2025-10-05: Refatoração completa da documentação baseada no código real (BackofficeUserResource e BackofficeUserCollection)
- 2025-10-04: Documento inicial
