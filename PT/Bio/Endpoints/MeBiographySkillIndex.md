# Bio – Minhas Habilidades de Biografia Índice

## Endpoint

```
GET /api/v1/bio/me/biography-skills
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
  "https://sandbox.your-domain.com/api/v1/bio/me/biography-skills"
```

### Exemplo de resposta

```json
{
  "data": ["uuid": "550e8400", "skill": "Python", "proficiency": "expert"]
}
```

## Status HTTP

- 200: Sucesso
- 401: Não autenticado
- 500: Erro interno

## Notas

- Retorna itens apenas para o usuário autenticado.

## Relacionados

- [Minhas Habilidades de Biografia — Criar](MyBiographySkillsCriar.md)
