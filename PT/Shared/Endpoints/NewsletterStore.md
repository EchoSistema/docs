# Shared – Inscrever-se na Newsletter

## Endpoint

```
POST /api/v1/newsletters
```

## Autenticação

Nenhuma

## Cabeçalhos

| Cabeçalho     | Tipo | Obrigatório | Descrição |
| ---------------- | ------ | -------- | ----------- |
| Authorization    | string | Não | Credencial `Bearer {token}`. |
| X-PUBLIC-KEY     | string | Sim      | Chave pública da plataforma. |
| Accept-Language  | string | Não       | Locale IETF (ex.: `pt-BR`, `en`, `es`). |

## Parâmetros

Inscrever um email na newsletter

## Exemplos

### Exemplo de requisição (curl)

```bash
curl -X POST \
  
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: pt-BR" \
  "https://sandbox.your-domain.com/api/v1/newsletters"
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

- Inscrever um email na newsletter

## Relacionados

- See other Shared API endpoints

## Changelog

- 2025-10-16: Documentação inicial
