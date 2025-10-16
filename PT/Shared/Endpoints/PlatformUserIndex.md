# Shared – Listar Usuários da Plataforma

## Endpoint

```
GET /api/v1/platform/users
```

## Autenticação

Obrigatória – Bearer {token}

## Cabeçalhos

| Cabeçalho     | Tipo | Obrigatório | Descrição |
| ---------------- | ------ | -------- | ----------- |
| Authorization    | string | Quando exigido | Credencial `Bearer {token}`. |
| X-PUBLIC-KEY     | string | Sim      | Chave pública da plataforma. |
| Accept-Language  | string | Não       | Locale IETF (ex.: `pt-BR`, `en`, `es`). |

## Parâmetros

Listar usuários da plataforma atual com filtros

## Exemplos

### Exemplo de requisição (curl)

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: pt-BR" \
  "https://sandbox.your-domain.com/api/v1/platform/users"
```

### Exemplo de resposta

```json
{
  "data": {}
}
```

## Estrutura JSON Explicada

| Campo | Tipo | Descrição |
| ----------- | ------- | ----------- |
| data        | object  | Response data |

## Status HTTP

- 200: OK
- 201: Criado
- 400: Requisição inválida
- 401: Não autenticado
- 403: Proibido
- 404: Não encontrado
- 422: Erro de validação
- 429: Limite de requests excedido
- 500: Erro interno

## Erros

```json
{
  "message": "Mensagem de erro"
}
```

## Notas

- Listar usuários da plataforma atual com filtros

## Relacionados

- See other Shared API endpoints

## Changelog

- 2025-10-16: Documentação inicial
