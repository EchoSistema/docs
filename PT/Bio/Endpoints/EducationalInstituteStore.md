# Bio – Educational Institutes Criar

## Endpoint

```
POST /api/v1/bio/educational-institutes
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
  "name": "MIT",
  "country": "US"
}
```

| Field   | Tipo   | Obrigatório | Descrição |
| ------- | ------ | -------- | ----------- |
| name    | string | Yes      | Institute name. |
| country | string | Yes      | Country code (ISO 2-letter). |

> Os parâmetros aceitam camelCase, snake_case, kebab-case, or CapitalCase variants.

## Exemplos

### Exemplo de requisição (curl)

```bash
curl -X POST \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Content-Type: application/json" \
  -d '{"name": "MIT", "country": "US"}' \
  "https://sandbox.your-domain.com/api/v1/bio/educational-institutes"
```

### Exemplo de resposta

```json
{
  "data": {
    "id": 1,
    "uuid": "550e8400-e29b-41d4-a716-446655440000",
    "name": "MIT",
    "country": "US",
    "created_at": "2025-01-15T10:30:00.000000Z",
    "updated_at": "2025-01-15T10:30:00.000000Z"
  }
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
    "name": ["The name field is required."]
  }
}
```

## Notas

- The authenticated user becomes the creator.

## Relacionados

- [Educational Institutes — Índice](EducationalInstituteÍndice.md)
- [Educational Institutes — Atualizar](EducationalInstituteAtualizar.md)
