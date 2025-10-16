# Bio – Minhas Experiências Profissionais Excluir

## Endpoint

```
DELETE /api/v1/bio/me/job-experiences/{uuid}
```

## Autenticação

Obrigatória – Bearer {token} com habilidade `backoffice` e permissão `domain:bio`.

## Cabeçalhos

| Cabeçalho           | Tipo   | Obrigatório | Descrição |
| ---------------- | ------ | -------- | ----------- |
| Authorization    | string | Yes      | `Bearer {token}`. |
| X-PUBLIC-KEY     | string | Yes      | Chave pública da plataforma. |

## Status HTTP

- 200: Sucesso
- 401: Não autenticado
- 404: Não encontrado
- 500: Erro interno

## Relacionados

- [Minhas Experiências Profissionais — Índice](MyJobExperiencesÍndice.md)
