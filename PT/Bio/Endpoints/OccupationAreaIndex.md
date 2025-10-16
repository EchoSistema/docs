# Bio – Occupation Areas Índice

## Endpoint

```
GET /api/v1/bio/occupation-areas
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
| perPage    | integer | No       | Número de itens por página. | 50 |

> Os parâmetros aceitam camelCase, snake_case, kebab-case, or CapitalCase variants.

## Exemplos

### Exemplo de requisição (curl)

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: en" \
  "https://sandbox.your-domain.com/api/v1/bio/occupation-areas"
```

### Exemplo de resposta

```json
{
  "data": [
    {
      "id": 1,
      "uuid": "550e8400-e29b-41d4-a716-446655440000",
      "titles": [
        {
          "id": 10,
          "content": "Software Development",
          "language": "en"
        },
        {
          "id": 11,
          "content": "Desenvolvimento de Software",
          "language": "pt-BR"
        }
      ],
      "created_at": "2025-01-15T10:30:00.000000Z",
      "updated_at": "2025-01-15T10:30:00.000000Z"
    }
  ],
  "links": {
    "first": "https://sandbox.your-domain.com/api/v1/bio/occupation-areas?page=1",
    "last": "https://sandbox.your-domain.com/api/v1/bio/occupation-areas?page=5",
    "prev": null,
    "next": "https://sandbox.your-domain.com/api/v1/bio/occupation-areas?page=2"
  },
  "meta": {
    "current_page": 1,
    "from": 1,
    "last_page": 5,
    "per_page": 50,
    "to": 50,
    "total": 250
  }
}
```

## JSON Structure Explained

| Field                  | Tipo    | Descrição |
| ---------------------- | ------- | ----------- |
| data[]                 | array   | List of occupation areas. |
| data[].id              | integer | Occupation area identifier. |
| data[].uuid            | string  | Unique occupation area identifier. |
| data[].titles[]        | array   | Multi-language titles. |
| data[].titles[].id     | integer | Title identifier. |
| data[].titles[].content| string  | Title in specific language. |
| data[].titles[].language| string | Language code. |
| data[].created_at      | string  | Creation timestamp. |
| data[].updated_at      | string  | Last update timestamp. |
| links                  | object  | Pagination links (when paginated). |
| meta                   | object  | Pagination metadata (when paginated). |

## Status HTTP

- 200: Sucesso
- 401: Não autenticado
- 403: Proibido
- 429: Limite de requisições excedido
- 500: Erro interno

## Erros

Standard error responses:

```json
{
  "message": "Unauthenticated."
}
```

## Notas

- Occupation areas are returned with all available language translations.
- Use `noPaginate=true` to retrieve all records at once.
- Default pagination is 50 items per page.

## Relacionados

- [Occupation Areas — Criar](OccupationAreaCriar.md)
- [Occupation Areas — Atualizar](OccupationAreaAtualizar.md)
- [Occupation Areas — Excluir](OccupationAreaExcluir.md)
