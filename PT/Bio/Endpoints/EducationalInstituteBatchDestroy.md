# Bio – Educational Institutes Batch Excluir

## Endpoint

```
DELETE /api/v1/bio/educational-institutes
```

## Autenticação

Obrigatória – Bearer {token} com habilidade `backoffice` e permissão `domain:bio`.

## Cabeçalhos

| Cabeçalho           | Tipo   | Obrigatório | Descrição |
| ---------------- | ------ | -------- | ----------- |
| Authorization    | string | Yes      | `Bearer {token}`. |
| X-PUBLIC-KEY     | string | Yes      | Chave pública da plataforma. |
| Content-Type     | string | Yes      | `application/json`. |

## Parâmetros

### Corpo da requisição

```json
{
  "institutes": [
    "550e8400-e29b-41d4-a716-446655440000",
    "660e8400-e29b-41d4-a716-446655440001"
  ]
}
```

| Field      | Tipo  | Obrigatório | Descrição |
| ---------- | ----- | -------- | ----------- |
| institutes | array | Yes      | Array of educational institute UUIDs. |

## Exemplos

### Exemplo de requisição (curl)

```bash
curl -X DELETE \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Content-Type: application/json" \
  -d '{"institutes": ["550e8400-e29b-41d4-a716-446655440000"]}' \
  "https://sandbox.your-domain.com/api/v1/bio/educational-institutes"
```

### Exemplo de resposta

```json
{
  "success": true,
  "message": "2 educational institutes deleted successfully."
}
```

## Status HTTP

- 200: Sucesso
- 401: Não autenticado
- 422: Erro de validação
- 500: Erro interno

## Erros

```json
{
  "message": "The given data was invalid.",
  "errors": {
    "institutes": ["The institutes field is required."]
  }
}
```

## Notas

- Only items created by the authenticated user can be deleted.
- Items not found or not owned are silently skipped.

## Relacionados

- [Educational Institutes — Índice](EducationalInstituteÍndice.md)
- [Educational Institutes — Excluir](EducationalInstituteExcluir.md)
