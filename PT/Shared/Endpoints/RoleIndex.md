# Shared – Listar Papéis Disponíveis

## Endpoint

```
GET /api/v1/roles
```

## Autenticação

Nenhuma

## Cabeçalhos

| Cabeçalho     | Tipo | Obrigatório | Descrição |
| ---------------- | ------ | -------- | ----------- |
| Authorization    | string | Não | Credencial `Bearer {token}`. |
| X-PUBLIC-KEY     | string | Sim      | Chave pública da plataforma. |
| Accept-Language  | string | Não       | Locale IETF (ex.: `pt-BR`, `en`, `es`). |

## Parâmetros de Query

| Parâmetro | Tipo | Obrigatório | Descrição |
| --------- | ---- | ----------- | ----------- |
| roles | array | Não | Array com nomes específicos de papéis a incluir (ex.: `administrator`, `supervisor`) |
| roles.* | enum | Não | Deve ser um valor válido do RoleEnum |
| except | array | Não | Array com nomes de papéis a excluir da resposta |
| counting | array | Não | Array de relacionamentos a contar (ex.: `["users", "platforms"]`) |
| permissions | boolean | Não | Incluir permissões do papel na resposta (padrão: false) |

## Exemplos

### Exemplo 1: Requisição básica (curl)

```bash
curl -X GET \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: pt-BR" \
  "https://sandbox.your-domain.com/api/v1/roles"
```

### Exemplo 2: Com contagem (curl)

```bash
curl -X GET \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: pt-BR" \
  "https://sandbox.your-domain.com/api/v1/roles?counting[]=users&counting[]=platforms&permissions=1"
```

### Exemplo 3: Filtrar papéis específicos (curl)

```bash
curl -X GET \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: pt-BR" \
  "https://sandbox.your-domain.com/api/v1/roles?roles[]=administrator&roles[]=supervisor&except[]=guest"
```

### Exemplo de resposta (básico)

```json
{
  "data": [
    {
      "id": 1,
      "uuid": "550e8400-e29b-41d4-a716-446655440000",
      "name": "administrator",
      "title": "Administrador",
      "created_at": "2024-01-15T10:30:00.000000Z"
    },
    {
      "id": 2,
      "uuid": "660e8400-e29b-41d4-a716-446655440001",
      "name": "supervisor",
      "title": "Supervisor",
      "created_at": "2024-01-15T10:30:00.000000Z"
    }
  ]
}
```

### Exemplo de resposta (com contagem e permissões)

```json
{
  "data": [
    {
      "id": 1,
      "uuid": "550e8400-e29b-41d4-a716-446655440000",
      "name": "administrator",
      "title": "Administrador",
      "created_at": "2024-01-15T10:30:00.000000Z",
      "users_count": 150,
      "platforms_count": 12,
      "permissions": [
        "backoffice.all",
        "platform.manage",
        "users.manage"
      ]
    }
  ]
}
```

## Estrutura JSON Explicada

| Campo | Tipo | Descrição |
| ----------- | ------- | ----------- |
| data | array | Array de objetos de papéis |
| data[].id | integer | Identificador único do papel |
| data[].uuid | string | UUID do papel |
| data[].name | string | Nome do sistema do papel |
| data[].title | string | Nome localizado do papel para exibição |
| data[].created_at | string | Timestamp ISO 8601 |
| data[].users_count | integer | Contagem de usuários com este papel (somente quando `counting` inclui "users") |
| data[].platforms_count | integer | Contagem de plataformas usando este papel (somente quando `counting` inclui "platforms") |
| data[].permissions | array | Array de strings de permissões (somente quando `permissions=1`) |

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

- O endpoint retorna papéis padrão: `administrator`, `supervisor`, `coordinator`, `support`, `guest`
- Papéis adicionais podem ser solicitados via parâmetro `roles`
- Papéis específicos podem ser excluídos usando o parâmetro `except`
- O parâmetro `counting` habilita contagens de relacionamentos (users, platforms)
- Comportamento específico por domínio: domínios ReputationBook e Intelligence suportam `withCount`, outros retornam todos os papéis
- Use `permissions=1` para incluir a lista completa de permissões de cada papel

## Relacionados

- Veja outros endpoints da API Shared
- Permissões de papéis são definidas em `Domain\Shared\Enums\Role\RoleEnum`

## Changelog

- 2025-11-20: Adicionados parâmetros de contagem, filtragem e documentação detalhada
- 2025-10-16: Documentação inicial
