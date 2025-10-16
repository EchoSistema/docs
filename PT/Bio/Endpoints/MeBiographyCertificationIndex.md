# Bio – Minhas Certificações Índice

## Endpoint

```
GET /api/v1/bio/me/certifications
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
  "https://sandbox.your-domain.com/api/v1/bio/me/certifications"
```

### Exemplo de resposta

```json
{
  "data": [
    {"uuid": "550e8400-e29b-41d4-a716-446655440000", "title": "AWS Certified", "issued_date": "2024-01-15"}
  ],
  "links": {
    "first": "https://sandbox.your-domain.com/api/v1/bio/me/certifications?page=1",
    "last": "https://sandbox.your-domain.com/api/v1/bio/me/certifications?page=5",
    "prev": null,
    "next": "https://sandbox.your-domain.com/api/v1/bio/me/certifications?page=2"
  },
  "meta": {
    "current_page": 1,
    "per_page": 25,
    "total": 100
  }
}
```

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

- Returns certifications for the authenticated user only.
- Use `noPaginate=true` to retrieve all records at once.
- Default pagination is 25 items per page.

## Relacionados

- [Minhas Certificações — Mostrar](MyCertificationsMostrar.md)
- [Minhas Certificações — Criar](MyCertificationsCriar.md)
- [Minhas Certificações — Atualizar](MyCertificationsAtualizar.md)
- [Minhas Certificações — Excluir](MyCertificationsExcluir.md)
