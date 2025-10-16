# Bio – Minhas Educações Batch Excluir

## Endpoint

```
DELETE /api/v1/bio/me/educations
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
  "educations": ["uuid1", "uuid2"]
}
```

## Status HTTP

- 200: Sucesso
- 401: Não autenticado
- 422: Erro de validação
- 500: Erro interno

## Relacionados

- [Minhas Educações — Índice](MyEducationsÍndice.md)
