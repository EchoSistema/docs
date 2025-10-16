# Bio – Occupation Areas Excluir

## Endpoint

```
DELETE /api/v1/bio/occupation-areas/{uuid}
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
| uuid      | string | Yes      | Occupation area UUID. |

## Exemplos

### Exemplo de requisição (curl)

```bash
curl -X DELETE \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  "https://sandbox.your-domain.com/api/v1/bio/occupation-areas/550e8400-e29b-41d4-a716-446655440000"
```

### Exemplo de resposta

```json
{
  "success": true,
  "message": "Occupation area 'Software Development' deleted successfully."
}
```

## JSON Structure Explained

| Field   | Tipo    | Descrição |
| ------- | ------- | ----------- |
| success | boolean | Operation success status. |
| message | string  | Success message. |

## Status HTTP

- 200: Sucesso
- 401: Não autenticado
- 403: Proibido
- 404: Não encontrado
- 429: Limite de requisições excedido
- 500: Erro interno

## Erros

Forbidden error example:

```json
{
  "message": "You are not authorized to delete this occupation area."
}
```

## Notas

- Only the creator can delete the occupation area.
- Deletion is permanent and cannot be undone.

## Relacionados

- [Occupation Areas — Índice](OccupationAreaÍndice.md)
- [Occupation Areas — Batch Excluir](OccupationAreaBatchExcluir.md)
