# Bio – Skills Batch Excluir

## Endpoint

```
DELETE /api/v1/bio/skills
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

### Corpo da requisição

```json
{
  "skills": [
    "550e8400-e29b-41d4-a716-446655440000",
    "660e8400-e29b-41d4-a716-446655440001"
  ]
}
```

| Field    | Tipo  | Obrigatório | Descrição |
| -------- | ----- | -------- | ----------- |
| skills   | array | Yes      | Array of skill UUIDs to delete. |

> Os parâmetros aceitam camelCase, snake_case, kebab-case, or CapitalCase variants.

## Exemplos

### Exemplo de requisição (curl)

```bash
curl -X DELETE \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: en" \
  -H "Content-Type: application/json" \
  -d '{
    "skills": [
      "550e8400-e29b-41d4-a716-446655440000",
      "660e8400-e29b-41d4-a716-446655440001"
    ]
  }' \
  "https://sandbox.your-domain.com/api/v1/bio/skills"
```

### Exemplo de resposta

```json
{
  "success": true,
  "message": "2 skills deleted successfully."
}
```

## JSON Structure Explained

| Field   | Tipo    | Descrição |
| ------- | ------- | ----------- |
| success | boolean | Operation success status. |
| message | string  | Success message with count of deleted skills. |

## Status HTTP

- 200: Sucesso
- 400: Requisição inválida
- 401: Não autenticado
- 403: Proibido
- 422: Erro de validação
- 429: Limite de requisições excedido
- 500: Erro interno

## Erros

Validation error example:

```json
{
  "message": "The given data was invalid.",
  "errors": {
    "skills": ["The skills field is required."]
  }
}
```

## Notas

- Only skills created by the authenticated user can be deleted.
- Skills not found or not owned by the user are silently skipped.
- The response message includes the count of successfully deleted skills.
- Deletion is permanent and cannot be undone.

## Relacionados

- [Skills — Índice](SkillÍndice.md)
- [Skills — Excluir](SkillExcluir.md)
