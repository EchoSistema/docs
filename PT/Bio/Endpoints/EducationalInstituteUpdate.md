# Bio – Educational Institutes Atualizar

## Endpoint

```
PUT /api/v1/bio/educational-institutes/{uuid}
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
| uuid      | string | Yes      | Educational institute UUID. |

### Corpo da requisição

```json
{
  "name": "Massachusetts Institute of Technology"
}
```

| Field   | Tipo   | Obrigatório | Descrição |
| ------- | ------ | -------- | ----------- |
| name    | string | No       | Institute name. |
| country | string | No       | Country code (ISO 2-letter). |

## Exemplos

### Exemplo de requisição (curl)

```bash
curl -X PUT \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Content-Type: application/json" \
  -d '{"name": "Massachusetts Institute of Technology"}' \
  "https://sandbox.your-domain.com/api/v1/bio/educational-institutes/550e8400-e29b-41d4-a716-446655440000"
```

### Exemplo de resposta

```json
{
  "data": {
    "id": 1,
    "uuid": "550e8400-e29b-41d4-a716-446655440000",
    "name": "Massachusetts Institute of Technology",
    "country": "US",
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
  "message": "You are not authorized to update this educational institute."
}
```

## Notas

- Only the creator can update the educational institute.

## Relacionados

- [Educational Institutes — Índice](EducationalInstituteÍndice.md)
- [Educational Institutes — Excluir](EducationalInstituteExcluir.md)
