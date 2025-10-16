# Bio – Minhas Certificações Mostrar

## Endpoint

```
GET /api/v1/bio/me/certifications/{certification}
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

### Parâmetros de caminho

| Parâmetro | Tipo   | Obrigatório | Descrição |
| --------- | ------ | -------- | ----------- |
| certification | string | Yes      | UUID do recurso. |

## Exemplos

### Exemplo de requisição (curl)

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  "https://sandbox.your-domain.com/api/v1/bio/me/certifications/550e8400-e29b-41d4-a716-446655440000"
```

### Exemplo de resposta

```json
{
  "data": {"uuid": "550e8400-e29b-41d4-a716-446655440000", "title": "AWS Certified", "issued_date": "2024-01-15"}
}
```

## Status HTTP

- 200: Sucesso
- 401: Não autenticado
- 403: Proibido
- 404: Não encontrado
- 500: Erro interno

## Erros

```json
{
  "message": "Resource not found."
}
```

## Notas

- Only returns items owned by the authenticated user.

## Relacionados

- [Minhas Certificações — Índice](MyCertificationsÍndice.md)
- [Minhas Certificações — Atualizar](MyCertificationsAtualizar.md)
- [Minhas Certificações — Excluir](MyCertificationsExcluir.md)
