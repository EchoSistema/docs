# Bio – Occupation Areas Batch Excluir

## Endpoint

```
DELETE /api/v1/bio/occupation-areas
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
  "areas": [
    "550e8400-e29b-41d4-a716-446655440000",
    "660e8400-e29b-41d4-a716-446655440001"
  ]
}
```

| Field  | Tipo  | Obrigatório | Descrição |
| ------ | ----- | -------- | ----------- |
| areas  | array | Yes      | Array of occupation area UUIDs to delete. |

> Os parâmetros aceitam camelCase, snake_case, kebab-case, or CapitalCase variants.

## Exemplos

### Exemplo de requisição (curl)

```bash
curl -X DELETE \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Content-Type: application/json" \
  -d '{
    "areas": [
      "550e8400-e29b-41d4-a716-446655440000",
      "660e8400-e29b-41d4-a716-446655440001"
    ]
  }' \
  "https://sandbox.your-domain.com/api/v1/bio/occupation-areas"
```

### Exemplo de resposta

```json
{
  "success": true,
  "message": "2 occupation areas deleted successfully."
}
```

## JSON Structure Explained

| Field   | Tipo    | Descrição |
| ------- | ------- | ----------- |
| success | boolean | Operation success status. |
| message | string  | Success message with count. |

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
    "areas": ["The areas field is required."]
  }
}
```

## Notas

- Only occupation areas created by the authenticated user can be deleted.
- Items not found or not owned by the user are silently skipped.
- The response includes the count of successfully deleted items.

## Relacionados

- [Occupation Areas — Índice](OccupationAreaÍndice.md)
- [Occupation Areas — Excluir](OccupationAreaExcluir.md)
