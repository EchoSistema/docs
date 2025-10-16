# Bio – Minhas Experiências Profissionais Batch Excluir

## Endpoint

```
DELETE /api/v1/bio/me/job-experiences
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
  "job_experiences": ["uuid1", "uuid2"]
}
```

## Status HTTP

- 200: Sucesso
- 401: Não autenticado
- 422: Erro de validação
- 500: Erro interno

## Relacionados

- [Minhas Experiências Profissionais — Índice](MyJobExperiencesÍndice.md)
