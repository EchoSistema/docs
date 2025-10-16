# Bio – Minha Biografia Mostrar

## Endpoint

```
GET /api/v1/bio/me
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

None.

## Exemplos

### Exemplo de requisição (curl)

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: en" \
  "https://sandbox.your-domain.com/api/v1/bio/me"
```

### Exemplo de resposta

```json
{
  "data": {
    "id": 1,
    "uuid": "550e8400-e29b-41d4-a716-446655440000",
    "user_id": 123,
    "slug": "john-doe",
    "public_email": "john@example.com",
    "summary": "Software engineer with 10 years of experience",
    "headline": "Senior Software Engineer",
    "created_at": "2025-01-15T10:30:00.000000Z",
    "updated_at": "2025-01-15T10:30:00.000000Z",
    "user": {
      "id": 123,
      "name": "John Doe",
      "email": "john@example.com"
    }
  }
}
```

## JSON Structure Explained

| Field           | Tipo    | Descrição |
| --------------- | ------- | ----------- |
| data            | object  | Biography object. |
| data.id         | integer | Biography identifier. |
| data.uuid       | string  | Unique biography identifier. |
| data.user_id    | integer | Associated user ID. |
| data.slug       | string  | URL-friendly slug. |
| data.public_email| string | Public email address. |
| data.summary    | string  | Biography summary. |
| data.headline   | string  | Professional headline. |
| data.user       | object  | Associated user object. |

## Status HTTP

- 200: Sucesso
- 401: Não autenticado
- 403: Proibido
- 500: Erro interno

## Erros

```json
{
  "message": "Unauthenticated."
}
```

## Notas

- Returns the biography of the authenticated user.
- If no biography exists, a default biography object is created and returned.

## Relacionados

- [Minha Biografia — Atualizar](MeBiographyAtualizar.md)
