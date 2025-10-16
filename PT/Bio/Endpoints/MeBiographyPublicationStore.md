# Bio – Minhas Publicações Criar

## Endpoint

```
POST /api/v1/bio/me/publications
```

## Autenticação

Obrigatória – Bearer {token} com habilidade `backoffice` e permissão `domain:bio`.

## Cabeçalhos

| Cabeçalho           | Tipo   | Obrigatório | Descrição |
| ---------------- | ------ | -------- | ----------- |
| Authorization    | string | Yes      | `Bearer {token}`. |
| X-PUBLIC-KEY     | string | Yes      | Chave pública da plataforma. |
| Content-Type     | string | Yes      | `application/json`. |

## Status HTTP

- 201: Criado
- 401: Não autenticado
- 422: Erro de validação
- 500: Erro interno

## Relacionados

- [Minhas Publicações — Índice](MyPublicationsÍndice.md)
