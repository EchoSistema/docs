# Bio – Minhas Certificações Criar

## Endpoint

```
POST /api/v1/bio/me/certifications
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
{"title": "AWS Certified", "issued_date": "2024-01-15", "issuer": "Amazon"}
```

> Os parâmetros aceitam camelCase, snake_case, kebab-case, or CapitalCase variants.

## Exemplos

### Exemplo de requisição (curl)

```bash
curl -X POST \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Content-Type: application/json" \
  -d '{"title": "AWS Certified", "issued_date": "2024-01-15", "issuer": "Amazon"}' \
  "https://sandbox.your-domain.com/api/v1/bio/me/certifications"
```

### Exemplo de resposta

```json
{
  "data": {"uuid": "550e8400-e29b-41d4-a716-446655440000", "title": "AWS Certified"}
}
```

## Status HTTP

- 201: Criado
- 400: Requisição inválida
- 401: Não autenticado
- 422: Erro de validação
- 500: Erro interno

## Erros

```json
{
  "message": "The given data was invalid.",
  "errors": {
    "field": ["Validation error message."]
  }
}
```

## Notas

- Creates a new resource for the authenticated user.

## Relacionados

- [Minhas Certificações — Índice](MyCertificationsÍndice.md)
- [Minhas Certificações — Mostrar](MyCertificationsMostrar.md)
- [Minhas Certificações — Atualizar](MyCertificationsAtualizar.md)
