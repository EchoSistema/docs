# Bio – Occupation Areas Atualizar

## Endpoint

```
PUT /api/v1/bio/occupation-areas/{uuid}
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

### Parâmetros de caminho

| Parâmetro | Tipo   | Obrigatório | Descrição |
| --------- | ------ | -------- | ----------- |
| uuid      | string | Yes      | Occupation area UUID. |

### Corpo da requisição

```json
{
  "titles": [
    {
      "content": "Software Engineering",
      "language": "en"
    }
  ]
}
```

| Field             | Tipo   | Obrigatório | Descrição |
| ----------------- | ------ | -------- | ----------- |
| titles            | array  | Yes      | Array of titles in different languages. |
| titles[].content  | string | Yes      | Title text. |
| titles[].language | string | Yes      | Language code (e.g., `en`, `pt-BR`). |

> Os parâmetros aceitam camelCase, snake_case, kebab-case, or CapitalCase variants.

## Exemplos

### Exemplo de requisição (curl)

```bash
curl -X PUT \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Content-Type: application/json" \
  -d '{
    "titles": [
      {"content": "Software Engineering", "language": "en"}
    ]
  }' \
  "https://sandbox.your-domain.com/api/v1/bio/occupation-areas/550e8400-e29b-41d4-a716-446655440000"
```

### Exemplo de resposta

```json
{
  "data": {
    "id": 1,
    "uuid": "550e8400-e29b-41d4-a716-446655440000",
    "titles": [
      {
        "id": 10,
        "content": "Software Engineering",
        "language": "en"
      }
    ],
    "created_at": "2025-01-15T10:30:00.000000Z",
    "updated_at": "2025-01-15T12:00:00.000000Z"
  }
}
```

## JSON Structure Explained

| Field                  | Tipo    | Descrição |
| ---------------------- | ------- | ----------- |
| data                   | object  | Atualizard occupation area. |
| data.id                | integer | Occupation area identifier. |
| data.uuid              | string  | Unique occupation area identifier. |
| data.titles[]          | array   | Multi-language titles. |
| data.created_at        | string  | Creation timestamp. |
| data.updated_at        | string  | Last update timestamp. |

## Status HTTP

- 200: Sucesso
- 401: Não autenticado
- 403: Proibido
- 404: Não encontrado
- 422: Erro de validação
- 429: Limite de requisições excedido
- 500: Erro interno

## Erros

Forbidden error example:

```json
{
  "message": "You are not authorized to update this occupation area."
}
```

## Notas

- Only the creator can update the occupation area.
- Existing titles can be updated and new translations can be added.

## Relacionados

- [Occupation Areas — Índice](OccupationAreaÍndice.md)
- [Occupation Areas — Criar](OccupationAreaCriar.md)
- [Occupation Areas — Excluir](OccupationAreaExcluir.md)
