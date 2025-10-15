# Chat – Listar Mensagens de uma Conversa

## Endpoint

`GET /api/v1/chat/conversations/{uuid}/messages`

Recupera todas as mensagens de uma conversa específica, incluindo informações da conversa e seu contexto associado.

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

| Parâmetro | Tipo   | Obrigatório | Descrição |
| --------- | ------ | ----------- | --------- |
| `uuid`    | `uuid` | Sim         | UUID da conversa. |

---

## Exemplos

### Exemplo de Requisição (curl)

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <chave>" \
  "https://sandbox.seu-dominio.com/api/v1/chat/conversations/9b732d6a-2e56-4db8-9b2e-6fd8c14e7f28/messages"
```

### Exemplo de Resposta

```json
{
  "data": {
    "conversation": {
      "uuid": "9b732d6a-2e56-4db8-9b2e-6fd8c14e7f28",
      "taggable_type": "App\\Models\\Order",
      "last_message_text": "Olá, tudo bem?",
      "last_message_at": "2025-10-15T10:30:00.000000Z",
      "is_archived": false,
      "created_at": "2025-10-01T08:00:00.000000Z",
      "updated_at": "2025-10-15T10:30:00.000000Z",
      "taggable_info": {
        "type": "Order",
        "uuid": "8a621c5b-1d45-4cb7-8a1d-5ec7b13d6e17",
        "title": "Pedido #1234"
      }
    },
    "messages": [
      {
        "message_id": "670e3a1b8f9c2d001e4f5a6b",
        "conversation_uuid": "9b732d6a-2e56-4db8-9b2e-6fd8c14e7f28",
        "sender": {
          "uuid": "7c531b4a-1c34-4ba6-7b0c-4db6a12c5d16",
          "name": "João Silva",
          "avatar": "https://cdn.example.com/avatars/joao.jpg"
        },
        "receiver": {
          "uuid": "6b420a39-0b23-3a95-6a0b-3ca5900b4c05",
          "name": "Maria Santos",
          "avatar": "https://cdn.example.com/avatars/maria.jpg"
        },
        "context": {
          "text": "Olá, tudo bem?",
          "type": "text"
        },
        "attachments": [],
        "metadata": {},
        "reactions": [
          {
            "user_uuid": "6b420a39-0b23-3a95-6a0b-3ca5900b4c05",
            "emoji": "👍",
            "created_at": "2025-10-15T10:31:00+00:00"
          }
        ],
        "status": {
          "is_read": true,
          "read_at": "2025-10-15T10:31:00+00:00",
          "delivered_at": "2025-10-15T10:30:00+00:00",
          "is_deleted": false
        },
        "timestamps": {
          "created_at": "2025-10-15T10:30:00+00:00",
          "updated_at": "2025-10-15T10:31:00+00:00"
        },
        "pbk": "platform_public_key",
        "device": {
          "type": "web",
          "os": "Windows",
          "os_version": "10",
          "app_version": "1.0.0",
          "model": null,
          "browser": "Chrome",
          "browser_version": "120.0"
        }
      }
    ]
  }
}
```

---

## Explicação da Estrutura JSON

### Resposta de Sucesso

| Campo  | Tipo     | Descrição |
| ------ | -------- | --------- |
| `data` | `object` | Contém a conversa e suas mensagens. |

### `data.conversation` – Objeto Conversa

| Campo               | Tipo      | Descrição |
| ------------------- | --------- | --------- |
| `uuid`              | `uuid`    | Identificador único da conversa. |
| `taggable_type`     | `string`  | Tipo do modelo polimórfico associado. |
| `last_message_text` | `string`  | Texto da última mensagem. |
| `last_message_at`   | `string`  | Data da última mensagem (formato ISO-8601). |
| `is_archived`       | `boolean` | Indica se a conversa está arquivada. |
| `created_at`        | `string`  | Data de criação da conversa (formato ISO-8601). |
| `updated_at`        | `string`  | Data da última atualização (formato ISO-8601). |
| `taggable_info`     | `object`  | Informações do contexto associado. |

### `data.messages[]` – Array de Mensagens

| Campo                | Tipo     | Descrição |
| -------------------- | -------- | --------- |
| `message_id`         | `string` | ID único da mensagem (MongoDB ObjectId). |
| `conversation_uuid`  | `uuid`   | UUID da conversa. |
| `sender`             | `object` | Informações do remetente. |
| `receiver`           | `object` | Informações do destinatário. |
| `context`            | `object` | Conteúdo da mensagem. |
| `attachments[]`      | `array`  | Lista de anexos (arquivos, imagens, etc.). |
| `metadata`           | `object` | Metadados adicionais. |
| `reactions[]`        | `array`  | Lista de reações (emojis). |
| `status`             | `object` | Status de entrega e leitura. |
| `timestamps`         | `object` | Timestamps de criação e atualização. |
| `pbk`                | `string` | Chave pública da plataforma. |
| `device`             | `object` | Informações do dispositivo de envio. |

### `sender` / `receiver` – Informações do Usuário

| Campo    | Tipo     | Descrição |
| -------- | -------- | --------- |
| `uuid`   | `uuid`   | UUID do usuário. |
| `name`   | `string` | Nome do usuário. |
| `avatar` | `string` | URL do avatar (pode ser null). |

### `context` – Conteúdo da Mensagem

| Campo  | Tipo     | Descrição |
| ------ | -------- | --------- |
| `text` | `string` | Texto da mensagem. |
| `type` | `string` | Tipo de conteúdo: `text`, `image`, `file`, `audio`, `video`. |

### `reactions[]` – Reações

| Campo        | Tipo     | Descrição |
| ------------ | -------- | --------- |
| `user_uuid`  | `uuid`   | UUID do usuário que reagiu. |
| `emoji`      | `string` | Emoji da reação. |
| `created_at` | `string` | Data da reação (formato ISO-8601). |

### `status` – Status da Mensagem

| Campo          | Tipo      | Descrição |
| -------------- | --------- | --------- |
| `is_read`      | `boolean` | Indica se a mensagem foi lida. |
| `read_at`      | `string`  | Data de leitura (formato ISO-8601, null se não lida). |
| `delivered_at` | `string`  | Data de entrega (formato ISO-8601). |
| `is_deleted`   | `boolean` | Indica se a mensagem foi deletada. |

### `device` – Informações do Dispositivo

| Campo             | Tipo     | Descrição |
| ----------------- | -------- | --------- |
| `type`            | `string` | Tipo do dispositivo: `mobile`, `web`, `desktop`. |
| `os`              | `string` | Sistema operacional. |
| `os_version`      | `string` | Versão do sistema operacional (pode ser null). |
| `app_version`     | `string` | Versão da aplicação (pode ser null). |
| `model`           | `string` | Modelo do dispositivo (pode ser null). |
| `browser`         | `string` | Navegador (pode ser null). |
| `browser_version` | `string` | Versão do navegador (pode ser null). |

---

## Status HTTP

- 200: Sucesso
- 401: Não autenticado
- 403: Proibido (usuário não é participante da conversa)
- 404: Conversa não encontrada
- 500: Erro interno

---

## Notas

* Apenas participantes da conversa podem visualizar suas mensagens.
* As mensagens são ordenadas por data de criação em ordem crescente (mais antigas primeiro).
* O campo `taggable_info` fornece contexto sobre o que originou a conversa (ex.: um pedido, uma reserva, etc.).
* Mensagens deletadas (`is_deleted: true`) ainda são retornadas, mas podem ser ocultadas no frontend.

---

## Relacionados

- [Listar Conversas](./ChatConversationsList.md)
- [Enviar Mensagem](./ChatMessageSend.md)
- [Marcar Mensagem como Lida](./ChatMessageMarkRead.md)
- [Adicionar Reação](./ChatMessageAddReaction.md)

---

## Changelog

- 2025-10-15: Criação inicial da documentação.
