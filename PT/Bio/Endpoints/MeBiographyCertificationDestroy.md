# Bio – Minhas Certificações Excluir

## Endpoint

```
DELETE /api/v1/bio/me/certifications/{certification}
```

## Autenticação

Obrigatória – Bearer {token} com habilidade `backoffice` e permissão `domain:bio`.

## Cabeçalhos

| Cabeçalho           | Tipo   | Obrigatório | Descrição |
| ---------------- | ------ | -------- | ----------- |
| Authorization    | string | Yes      | `Bearer {token}`. |
| X-PUBLIC-KEY     | string | Yes      | Chave pública da plataforma. |

## Parâmetros

### Parâmetros de caminho

| Parâmetro | Tipo   | Obrigatório | Descrição |
| --------- | ------ | -------- | ----------- |
| certification | string | Yes      | UUID do recurso. |

## Exemplos

### Exemplo de requisição (curl)

```bash
curl -X DELETE \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  "https://sandbox.your-domain.com/api/v1/bio/me/certifications/550e8400-e29b-41d4-a716-446655440000"
```

### Exemplo de resposta

```json
{
  "success": true,
  "message": "Certification deleted successfully."
}
```

## Status HTTP

- 200: Sucesso
- 401: Não autenticado
- 403: Proibido
- 404: Não encontrado
- 500: Erro interno

## Erros

```json
{
  "message": "You are not authorized to delete this resource."
}
```

## Notas

- Only the owner can delete the resource.
- Deletion is permanent and cannot be undone.

## Relacionados

- [Minhas Certificações — Índice](MyCertificationsÍndice.md)
- [Minhas Certificações — Batch Excluir](MyCertificationsBatchExcluir.md)
