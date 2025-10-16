# Bio – Minhas Certificações Atualizar

## Endpoint

```
PUT /api/v1/bio/me/certifications/{certification}
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

### Parâmetros de caminho

| Parâmetro | Tipo   | Obrigatório | Descrição |
| --------- | ------ | -------- | ----------- |
| certification | string | Yes      | UUID do recurso. |

### Corpo da requisição

```json
{"title": "AWS Certified Solutions Architect"}
```

> Os parâmetros aceitam camelCase, snake_case, kebab-case, or CapitalCase variants.

## Exemplos

### Exemplo de requisição (curl)

```bash
curl -X PUT \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Content-Type: application/json" \
  -d '{"title": "AWS Certified Solutions Architect"}' \
  "https://sandbox.your-domain.com/api/v1/bio/me/certifications/550e8400-e29b-41d4-a716-446655440000"
```

### Exemplo de resposta

```json
{
  "data": {
    "uuid": "550e8400-e29b-41d4-a716-446655440000",
    "updated_at": "2025-01-15T12:00:00.000000Z"
  }
}
```

## Status HTTP

- 200: Sucesso
- 401: Não autenticado
- 403: Proibido
- 404: Não encontrado
- 422: Erro de validação
- 500: Erro interno

## Erros

```json
{
  "message": "You are not authorized to update this resource."
}
```

## Notas

- Only the owner can update the resource.
- All fields in the request body are optional.

## Relacionados

- [Minhas Certificações — Índice](MyCertificationsÍndice.md)
- [Minhas Certificações — Mostrar](MyCertificationsMostrar.md)
- [Minhas Certificações — Excluir](MyCertificationsExcluir.md)
