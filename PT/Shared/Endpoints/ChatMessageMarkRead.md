# Chat – Marcar Mensagem como Lida

## Endpoint

`PATCH /api/v1/chat/messages/{messageId}/read`

Marca uma mensagem como lida, atualizando seu status e registrando a data/hora de leitura.

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

---

## Exemplos

### Exemplo de Requisição (curl)

```bash
curl -X PATCH \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <chave>" \
  "https://sandbox.seu-dominio.com/api/v1/chat/messages/670e3a1b8f9c2d001e4f5a6b/read"
```

### Exemplo de Resposta

```json
{
  "message": "Mensagem marcada como lida"
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
- 401: Não autenticado
- 403: Proibido (usuário não é o destinatário da mensagem)
- 404: Mensagem não encontrada
- 500: Erro interno

---

## Erros

### Exemplo de Erro de Autorização

```json
{
  "message": "Não autorizado"
}
```

---

## Notas

* Apenas o destinatário da mensagem (`receiver`) pode marcá-la como lida.
* A operação atualiza os campos `status.is_read` para `true` e `status.read_at` com a data/hora atual.
* O campo `timestamps.updated_at` também é atualizado.
* Se a mensagem já estiver marcada como lida, a operação ainda retornará sucesso.
* Esta ação pode ser usada para atualizar contadores de mensagens não lidas na interface do usuário.

---

## Relacionados

- [Listar Mensagens de uma Conversa](./ChatConversationMessages.md)
- [Enviar Mensagem](./ChatMessageSend.md)
- [Adicionar Reação](./ChatMessageAddReaction.md)

---

## Changelog

- 2025-10-15: Criação inicial da documentação.
