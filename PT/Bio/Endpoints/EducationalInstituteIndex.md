# Bio – Educational Institutes Índice

## Endpoint

```
GET /api/v1/bio/educational-institutes
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

### Parâmetros de consulta

| Parâmetro  | Tipo    | Obrigatório | Descrição | Padrão/Valores |
| ---------- | ------- | -------- | ----------- | -------------- |
| noPaginate | boolean | No       | Retornar todos os registros sem paginação. | false |
| perPage    | integer | No       | Número de itens por página. | 25 |

> Os parâmetros aceitam camelCase, snake_case, kebab-case, or CapitalCase variants.

## Exemplos

### Exemplo de requisição (curl)

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: en" \
  "https://sandbox.your-domain.com/api/v1/bio/educational-institutes"
```

### Exemplo de resposta

```json
{
  "data": [
    {
      "id": 1,
      "uuid": "550e8400-e29b-41d4-a716-446655440000",
      "name": "MIT",
      "country": "US",
      "created_at": "2025-01-15T10:30:00.000000Z",
      "updated_at": "2025-01-15T10:30:00.000000Z"
    }
  ],
  "links": {
    "first": "https://sandbox.your-domain.com/api/v1/bio/educational-institutes?page=1",
    "last": "https://sandbox.your-domain.com/api/v1/bio/educational-institutes?page=5",
    "prev": null,
    "next": "https://sandbox.your-domain.com/api/v1/bio/educational-institutes?page=2"
  },
  "meta": {
    "current_page": 1,
    "per_page": 25,
    "total": 125
  }
}
```

## JSON Structure Explained

| Field             | Tipo    | Descrição |
| ----------------- | ------- | ----------- |
| data[]            | array   | List of educational institutes. |
| data[].id         | integer | Institute identifier. |
| data[].uuid       | string  | Unique institute identifier. |
| data[].name       | string  | Institute name. |
| data[].country    | string  | Country code. |
| data[].created_at | string  | Creation timestamp. |
| data[].updated_at | string  | Last update timestamp. |

## Status HTTP

- 200: Sucesso
- 401: Não autenticado
- 403: Proibido
- 429: Limite de requisições excedido
- 500: Erro interno

## Erros

```json
{
  "message": "Unauthenticated."
}
```

## Notas

- Returns all educational institutes regardless of creator.
- Default pagination is 25 items per page.

## Relacionados

- [Educational Institutes — Mostrar](EducationalInstituteMostrar.md)
- [Educational Institutes — Criar](EducationalInstituteCriar.md)
