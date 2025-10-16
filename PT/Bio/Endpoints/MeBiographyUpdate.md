# Bio – Minha Biografia Atualizar

## Endpoint

```
PUT /api/v1/bio/me
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
  "slug": "john-doe-developer",
  "public_email": "john.public@example.com",
  "summary": "Experienced software engineer specializing in web development",
  "headline": "Senior Full-Stack Engineer"
}
```

| Field        | Tipo   | Obrigatório | Descrição |
| ------------ | ------ | -------- | ----------- |
| slug         | string | No       | URL-friendly slug. |
| public_email | string | No       | Public email address. |
| summary      | string | No       | Biography summary. |
| headline     | string | No       | Professional headline. |

> Os parâmetros aceitam camelCase, snake_case, kebab-case, or CapitalCase variants.

## Exemplos

### Exemplo de requisição (curl)

```bash
curl -X PUT \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Content-Type: application/json" \
  -d '{
    "slug": "john-doe-developer",
    "summary": "Experienced software engineer"
  }' \
  "https://sandbox.your-domain.com/api/v1/bio/me"
```

### Exemplo de resposta

```json
{
  "data": {
    "id": 1,
    "uuid": "550e8400-e29b-41d4-a716-446655440000",
    "user_id": 123,
    "slug": "john-doe-developer",
    "public_email": "john.public@example.com",
    "summary": "Experienced software engineer",
    "headline": "Senior Full-Stack Engineer",
    "updated_at": "2025-01-15T12:00:00.000000Z"
  }
}
```

## Status HTTP

- 200: Sucesso
- 401: Não autenticado
- 403: Proibido
- 422: Erro de validação
- 500: Erro interno

## Erros

```json
{
  "message": "The given data was invalid.",
  "errors": {
    "slug": ["The slug has already been taken."]
  }
}
```

## Notas

- All fields are optional in the update request.
- The slug is automatically generated from the provided value.
- Atualizars only the authenticated user's biography.

## Relacionados

- [Minha Biografia — Mostrar](MeBiographyMostrar.md)
