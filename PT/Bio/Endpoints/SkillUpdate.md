# Bio – Skills Atualizar

## Endpoint

```
PUT /api/v1/bio/skills/{uuid}
```

## Autenticação

Obrigatória – Bearer {token} com habilidade `backoffice` e permissão `domain:bio`.

## Cabeçalhos

| Cabeçalho           | Tipo   | Obrigatório | Descrição |
| ---------------- | ------ | -------- | ----------- |
| Authorization    | string | Yes      | `Bearer {token}`. |
| X-PUBLIC-KEY     | string | Yes      | Chave pública da plataforma. |
| Accept-Language  | string | No       | Locale IETF (e.g., `pt-BR`, `en`, `es`). |
| Content-Type     | string | Yes      | `application/json`. |

## Parâmetros

### Parâmetros de caminho

| Parâmetro | Tipo   | Obrigatório | Descrição |
| --------- | ------ | -------- | ----------- |
| uuid      | string | Yes      | Skill UUID. |

### Corpo da requisição

```json
{
  "content": "Python 3.x",
  "language": "en",
  "is_default": true
}
```

| Field      | Tipo    | Obrigatório | Descrição |
| ---------- | ------- | -------- | ----------- |
| content    | string  | No       | Skill name/title. |
| language   | string  | No       | Language code (e.g., `en`, `pt-BR`). |
| is_default | boolean | No       | Whether this is a default skill. |

> Os parâmetros aceitam camelCase, snake_case, kebab-case, or CapitalCase variants.

## Exemplos

### Exemplo de requisição (curl)

```bash
curl -X PUT \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: en" \
  -H "Content-Type: application/json" \
  -d '{
    "content": "Python 3.x",
    "is_default": true
  }' \
  "https://sandbox.your-domain.com/api/v1/bio/skills/550e8400-e29b-41d4-a716-446655440000"
```

### Exemplo de resposta

```json
{
  "data": {
    "id": 123,
    "uuid": "550e8400-e29b-41d4-a716-446655440000",
    "content": "Python 3.x",
    "language": "en",
    "usage": "skill",
    "is_default": true,
    "created_at": "2025-01-15T10:30:00.000000Z",
    "updated_at": "2025-01-15T11:45:00.000000Z"
  }
}
```

## JSON Structure Explained

| Field          | Tipo    | Descrição |
| -------------- | ------- | ----------- |
| data           | object  | Atualizard skill object. |
| data.id        | integer | Skill identifier. |
| data.uuid      | string  | Unique skill identifier. |
| data.content   | string  | Skill name/title. |
| data.language  | string  | Language code. |
| data.usage     | string  | Usage type (always "skill"). |
| data.is_default| boolean | Whether this is a default skill. |
| data.created_at| string  | Creation timestamp. |
| data.updated_at| string  | Last update timestamp. |

## Status HTTP

- 200: Sucesso
- 400: Requisição inválida
- 401: Não autenticado
- 403: Proibido
- 404: Não encontrado
- 422: Erro de validação
- 429: Limite de requisições excedido
- 500: Erro interno

## Erros

Not found error example:

```json
{
  "message": "Skill not found."
}
```

Forbidden error example (not creator):

```json
{
  "message": "You are not authorized to update this skill."
}
```

## Notas

- Only the creator of the skill can update it.
- All fields are optional in the update request.
- The `updated_at` timestamp is automatically updated.

## Relacionados

- [Skills — Índice](SkillÍndice.md)
- [Skills — Criar](SkillCriar.md)
- [Skills — Excluir](SkillExcluir.md)
