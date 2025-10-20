# Shared – Criar Mensagem de Contato

## Endpoint

`POST /api/v1/contact`

## Autenticação

Nenhuma (público). Requer cabeçalho de plataforma.

## Cabeçalhos

| Cabeçalho | Tipo | Obrigatório | Descrição |
| --------- | ---- | ----------- | --------- |
| X-PUBLIC-KEY | `string` | Sim | Chave pública da plataforma (também aceita `public_key` na query). |
| X-APP-NAME | `string` | Não | Quando coincide com o nome de uma plataforma, o reCAPTCHA não é exigido. |
| Accept-Language | `string` | Não | Locale para mensagens de validação (`pt-BR`, `en`, `es`, `gn`). |

## Parâmetros

Enviar uma mensagem de formulário de contato

### Corpo (application/json)

| Campo | Tipo | Obrigatório | Descrição |
| ----- | ---- | ----------- | --------- |
| `type` | `enum` | Sim | `default`, `denunciation` ou `report`. |
| `language` | `enum` | Sim | `pt-BR`, `en`, `es`, `gn`. |
| `g_recaptcha` | `string` | Obrigatório salvo `X-APP-NAME` | Token reCAPTCHA validado no servidor. |
| `email` | `email` | Obrigatório sem `phone` | E-mail do contato. |
| `phone` | `string` | Obrigatório sem `email` | Telefone do contato. |
| `content` | `string` | Sim | Mensagem (máx. 5000). Alias de `message` se enviado. |
| `name` | `string` | Não | Nome do remetente. |
| `user_uuid` | `uuid` | Não | Associa a um usuário existente. |
| `anonymous` | `boolean` | Não | Se `true`, usa `anonymous@pigeons.email`. |

## Exemplos

### Exemplo de requisição (curl)

```bash
curl -X POST \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: pt-BR" \
  -H "Content-Type: application/json" \
  "https://sandbox.your-domain.com/api/v1/contact" \
  -d '{
    "type": "default",
    "language": "pt-BR",
    "g_recaptcha": "<token>",
    "email": "maria@example.com",
    "content": "Gostaria de saber mais sobre os planos.",
    "name": "Maria Silva"
  }'
```

### Sucesso `201 Created`

```json
{
  "success": true,
  "data": { "uuid": "2c3f9d26-4c68-4478-8d1f-9a1ea3b6b32e" }
}
```

## Estrutura JSON Explicada

| Campo | Tipo | Descrição |
| ----- | ---- | --------- |
| success | `boolean` | Sempre `true` em sucesso. |
| data    | `object`  | Dados de resposta. |

## Status HTTP

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

- Enviar uma mensagem de formulário de contato

## Relacionados

- See other Shared API endpoints

## Changelog

- 2025-10-16: Documentação inicial
