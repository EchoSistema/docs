# Bio – Meus Livros Atualizar

## Endpoint

```
PUT /api/v1/bio/me/books/{uuid}
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

- 200: Sucesso
- 401: Não autenticado
- 404: Não encontrado
- 422: Erro de validação
- 500: Erro interno

## Relacionados

- [Meus Livros — Índice](MyBooksÍndice.md)
