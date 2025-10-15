# Chat – Adicionar Reação a Mensagem

## Endpoint

`POST /api/v1/chat/messages/{messageId}/reactions`

Adiciona uma reação (emoji) a uma mensagem existente.

---

## Autenticação

**Obrigatória** – Token Bearer.

---

## Requisição

### Cabeçalhos Obrigatórios

| Cabeçalho          | Tipo     | Descrição |
| ------------------ | -------- | --------- |
| **Authorization**  | `string` | Credencial `Bearer {token}`. **Obrigatório**. |
| **X-PUBLIC-KEY**   | `string` | Chave pública que identifica a plataforma solicitante. **Obrigatório**. |

### Parâmetros de Caminho

| Parâmetro   | Tipo     | Obrigatório | Descrição |
| ----------- | -------- | ----------- | --------- |
| `messageId` | `string` | Sim         | ID da mensagem (MongoDB ObjectId). |

### Parâmetros do Corpo da Requisição

| Parâmetro | Tipo     | Obrigatório | Descrição |
| --------- | -------- | ----------- | --------- |
| `emoji`   | `string` | Sim         | Emoji da reação (máx. 10 caracteres). |

---

## Exemplos

### Exemplo de Requisição (curl)

```bash
curl -X POST \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <chave>" \
  -H "Content-Type: application/json" \
  -d '{
    "emoji": "👍"
  }' \
  "https://sandbox.seu-dominio.com/api/v1/chat/messages/670e3a1b8f9c2d001e4f5a6b/reactions"
```

### Exemplo de Corpo da Requisição

```json
{
  "emoji": "👍"
}
```

### Exemplo de Resposta

```json
{
  "message": "Reação adicionada com sucesso"
}
```

---

## Explicação da Estrutura JSON

### Resposta de Sucesso

| Campo     | Tipo     | Descrição |
| --------- | -------- | --------- |
| `message` | `string` | Mensagem de confirmação. |

---

## Status HTTP

- 200: Sucesso
- 400: Requisição inválida
- 401: Não autenticado
- 404: Mensagem não encontrada
- 422: Erro de validação (emoji inválido ou muito longo)
- 500: Erro interno

---

## Erros

### Exemplo de Erro de Validação

```json
{
  "message": "The emoji field is required.",
  "errors": {
    "emoji": [
      "The emoji field is required."
    ]
  }
}
```

---

## Notas

* Qualquer usuário autenticado pode adicionar uma reação a uma mensagem.
* O mesmo usuário pode adicionar múltiplas reações (emojis diferentes) à mesma mensagem.
* A reação é adicionada ao array `reactions` da mensagem com o UUID do usuário, o emoji e a data/hora atual.
* O campo `timestamps.updated_at` da mensagem é atualizado.
* Não há validação se o usuário já adicionou o mesmo emoji anteriormente (permitindo duplicatas).
* O emoji é limitado a 10 caracteres para suportar emojis compostos.

---

## Relacionados

- [Listar Mensagens de uma Conversa](./ChatConversationMessages.md)
- [Enviar Mensagem](./ChatMessageSend.md)
- [Marcar Mensagem como Lida](./ChatMessageMarkRead.md)

---

## Changelog

- 2025-10-15: Criação inicial da documentação.
