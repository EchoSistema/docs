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
  "data": [
    {
      "conversation_uuid": "361be1bb-d7fb-4c3a-8a02-9071f69bacc8",
      "sender": {
        "uuid": "c1f1f474-4346-3118-83c5-2b5a6e6aff48",
        "name": "Ewerton Girardi",
        "avatar": "https://storage.echosistema.online/main/images/learn/forensic-academy/avatar/ewerton-daniel-080933300-1643321172.png"
      },
      "receiver": {
        "uuid": "05e12689-1a76-3af3-8326-7028e3b8a503",
        "name": "Daniel C. S. C. Soares",
        "avatar": "https://storage.echosistema.online/main/images/learn/forensic-academy/avatar/3-daniel-c-s-c-soares.png"
      },
      "context": {
        "text": "Olá! Como você está?",
        "texts": {
          "pt-BR": "Olá! Como você está?",
          "en": "Hello! How are you?"
        },
        "type": "text"
      },
      "attachments": [],
      "metadata": [],
      "reactions": [],
      "status": {
        "is_read": true,
        "read_at": "2025-10-15T09:36:04-03:00",
        "delivered_at": "2025-10-15T09:35:04-03:00",
        "is_deleted": false
      },
      "timestamps": {
        "created_at": "2025-10-15T09:34:04-03:00",
        "updated_at": "2025-10-15T09:34:04-03:00"
      },
      "pbk": "1396fd3b-d56b-362f-8ebb-c068bb910b69",
      "device": {
        "type": "mobile",
        "os": "macOS",
        "os_version": "11",
        "app_version": "1.0.0",
        "model": null,
        "browser": "Edge",
        "browser_version": "121.0"
      },
      "message_id": "68efa2cc35334e074c00abde",
      "id": "68efa2cc35334e074c00abde"
    }
  ],
  "conversation": {
    "uuid": "361be1bb-d7fb-4c3a-8a02-9071f69bacc8",
    "taggable_type": null,
    "last_message_text": "Valeu! Quando terminar aviso o time de QA.",
      "last_message_at": "2025-10-15T13:29:04.000000Z",
      "is_archived": false,
      "created_at": "2025-10-15T13:34:04.000000Z",
      "updated_at": "2025-10-15T13:34:04.000000Z",
      "taggable_info": null
  }
}
```

---

## Explicação da Estrutura JSON

### Corpo da Resposta

| Campo          | Tipo     | Descrição |
| -------------- | -------- | --------- |
| `data`         | `array`  | Mensagens pertencentes à conversa. |
| `conversation` | `object` | Conversa associada às mensagens listadas. |

### `conversation`

| Campo               | Tipo      | Descrição |
| ------------------- | --------- | --------- |
| `uuid`              | `uuid`    | Identificador único da conversa. |
| `taggable_type`     | `string`  | Tipo do modelo polimórfico associado. |
| `last_message_text` | `string`  | Texto da última mensagem. |
| `last_message_at`   | `string`  | Data da última mensagem (formato ISO-8601). |
| `is_archived`       | `boolean` | Indica se a conversa está arquivada. |
| `created_at`        | `string`  | Data de criação da conversa (formato ISO-8601). |
| `updated_at`        | `string`  | Data da última atualização (formato ISO-8601). |
| `taggable_info`     | `object`  | Informações do contexto associado (pode ser `null`). |

### `data[]` – Array de Mensagens

| Campo                | Tipo     | Descrição |
| -------------------- | -------- | --------- |
| `message_id`         | `string` | ID único da mensagem (MongoDB ObjectId). |
| `id`                 | `string` | Alias legado para `message_id`. |
| `conversation_uuid`  | `uuid`   | UUID da conversa. |
| `sender`             | `object` | Informações do remetente. |
| `receiver`           | `object` | Informações do destinatário. |
| `context`            | `object` | Conteúdo da mensagem. |
| `attachments`        | `array`  | Lista de anexos (pode estar vazia). |
| `metadata`           | `array`  | Metadados adicionais. |
| `reactions`          | `array`  | Lista de reações (emojis). |
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
| `text`  | `string`  | Texto principal da mensagem. |
| `texts` | `object?` | Traduções opcionais da mensagem por locale disponível. |
| `type`  | `string`  | Tipo de conteúdo: `text`, `image`, `file`, `audio`, `video`. |

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
* O campo `context.texts` é opcional e só aparece quando há traduções salvas para a mensagem.

---

## Relacionados

- [Listar Conversas](./ChatConversationsList.md)
- [Enviar Mensagem](./ChatMessageSend.md)
- [Marcar Mensagem como Lida](./ChatMessageMarkRead.md)
- [Adicionar Reação](./ChatMessageAddReaction.md)

---

## Changelog

- 2025-10-15: Criação inicial da documentação.
