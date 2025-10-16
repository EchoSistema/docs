# Bio – Skills Excluir

## Endpoint

```
DELETE /api/v1/bio/skills/{uuid}
```

## Autenticação

Obrigatória – Bearer {token} com habilidade `backoffice` e permissão `domain:bio`.

## Cabeçalhos

| Cabeçalho           | Tipo   | Obrigatório | Descrição |
| ---------------- | ------ | -------- | ----------- |
| Authorization    | string | Yes      | `Bearer {token}`. |
| X-PUBLIC-KEY     | string | Yes      | Chave pública da plataforma. |
| Accept-Language  | string | No       | Locale IETF (e.g., `pt-BR`, `en`, `es`). |

## Parâmetros

### Parâmetros de caminho

| Parâmetro | Tipo   | Obrigatório | Descrição |
| --------- | ------ | -------- | ----------- |
| uuid      | string | Yes      | Skill UUID. |

## Exemplos

### Exemplo de requisição (curl)

```bash
curl -X DELETE \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: en" \
  "https://sandbox.your-domain.com/api/v1/bio/skills/550e8400-e29b-41d4-a716-446655440000"
```

### Exemplo de resposta

```json
{
  "success": true,
  "message": "Skill 'Python' deleted successfully."
}
```

## JSON Structure Explained

| Field   | Tipo    | Descrição |
| ------- | ------- | ----------- |
| success | boolean | Operation success status. |
| message | string  | Success message with skill name. |

## Status HTTP

- 200: Sucesso
- 401: Não autenticado
- 403: Proibido
- 404: Não encontrado
- 429: Limite de requisições excedido
- 500: Erro interno

## Erros

Not found error example:

```json
{
  "message": "Skill not found."
}
```

Forbidden error example:

```json
{
  "message": "You are not authorized to delete this skill."
}
```

## Notas

- Only the creator of the skill can delete it.
- Deletion is permanent and cannot be undone.
- If the skill is referenced by user biographies, consider the cascade behavior.

## Relacionados

- [Skills — Índice](SkillÍndice.md)
- [Skills — Criar](SkillCriar.md)
- [Skills — Batch Excluir](SkillBatchExcluir.md)
