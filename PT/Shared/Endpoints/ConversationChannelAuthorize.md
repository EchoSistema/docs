# Shared – Autorizar Canal conversation.{uuid}

## Endpoint

```
POST /broadcasting/auth
```

## Autenticação

Obrigatória – Bearer {token}

## Cabeçalhos

| Cabeçalho       | Tipo   | Obrigatório | Descrição |
| --------------- | ------ | ----------- | --------- |
| Authorization   | string | Sim         | Credencial `Bearer {token}` do usuário autenticado. |
| X-PUBLIC-KEY    | string | Não         | Utilizado apenas quando a aplicação também exige chave pública. |
| Accept-Language | string | Não         | Locale IETF (`pt-BR`, `en`, `es`) para mensagens traduzíveis. |

## Parâmetros

### Parâmetros de caminho

Nenhum.

### Parâmetros de consulta

Nenhum.

### Corpo da requisição

| Campo          | Tipo   | Obrigatório | Descrição |
| -------------- | ------ | ----------- | --------- |
| channel_name   | string | Sim         | Deve seguir o padrão `conversation.{uuid}`, onde `{uuid}` é o identificador da conversa. |
| socket_id      | string | Sim         | Identificador de socket fornecido pelo Pusher/Echo na conexão WebSocket. |

```json
{
  "channel_name": "conversation.0f7d83cf-54b6-4ce0-9d33-8d6e9539d3c2",
  "socket_id": "1234.5678"
}
```

## Exemplos

### Exemplo de requisição (curl)

```bash
curl -X POST \
  -H "Authorization: Bearer <token>" \
  -H "Content-Type: application/json" \
  https://sandbox.exemplo.com/broadcasting/auth \
  -d '{
    "channel_name": "conversation.0f7d83cf-54b6-4ce0-9d33-8d6e9539d3c2",
    "socket_id": "1234.5678"
  }'
```

### Exemplo de resposta

```json
{
  "auth": "123456:abcdef0123456789",
  "channel_data": {
    "user_id": 42,
    "user_info": {
      "uuid": "b7c1e1f2-8f20-4e4b-a6e1-0bf8f2c8c7b3"
    }
  }
}
```

## Estrutura JSON Explicada

| Campo                     | Tipo   | Descrição |
| ------------------------- | ------ | --------- |
| auth                      | string | Token gerado pelo backend para validar a inscrição no canal privado. |
| channel_data              | object | Dados adicionais enviados para o provedor de broadcast (opcional). |
| channel_data.user_id      | integer| ID interno do usuário autenticado. |
| channel_data.user_info    | object | Metadados complementares do usuário. |
| channel_data.user_info.uuid | string | UUID do usuário autenticado. |

## Status HTTP

- 200: Autorização concedida
- 401: Token ausente ou inválido
- 403: Usuário não participa da conversa
- 404: Conversa não encontrada
- 429: Limite de requisições excedido
- 500: Erro interno inesperado

## Erros

```json
{
  "message": "Você não pertence a esta conversa.",
  "errors": null
}
```

## Notas

- Apenas participantes da conversa identificada pelo UUID podem ingressar no canal `conversation.{uuid}`.
- A regra reutiliza o relacionamento `participants` para validar o acesso de forma transacional.
- Certifique-se de sincronizar o UUID da conversa entre MySQL (tabela `conversations`) e o frontend.

## Relacionados

- Ainda não há endpoints relacionados documentados.

## Changelog

- 2025-10-15: Documento inicial.
