# Chat – Listagem de Conversas

## Endpoint

`GET /api/v1/chat/conversations`

Recupera uma lista de conversas do usuário autenticado, ordenadas pela data da última mensagem.

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

---

## Exemplo de Resposta

```json
{
  "data": [
    {
      "uuid": "9b732d6a-2e56-4db8-9b2e-6fd8c14e7f28",
      "taggable_type": "App\\Models\\Example",
      "last_message_text": "Olá, tudo bem?",
      "last_message_at": "2025-10-15T10:30:00.000000Z",
      "is_archived": false,
      "created_at": "2025-10-01T08:00:00.000000Z",
      "updated_at": "2025-10-15T10:30:00.000000Z",
      "taggable_info": {
        "type": "Example",
        "uuid": "8a621c5b-1d45-4cb7-8a1d-5ec7b13d6e17",
        "title": "Exemplo de Contexto"
      },
      "participants": [
        {
          "uuid": "7c531b4a-1c34-4ba6-7b0c-4db6a12c5d16",
          "name": "João Silva",
          "email": "joao@example.com",
          "pivot": {
            "conversation_id": 1,
            "user_id": 10,
            "unread_count": 2,
            "last_read_at": "2025-10-15T09:00:00.000000Z",
            "joined_at": "2025-10-01T08:00:00.000000Z",
            "created_at": "2025-10-01T08:00:00.000000Z",
            "updated_at": "2025-10-15T09:00:00.000000Z"
          }
        },
        {
          "uuid": "6b420a39-0b23-3a95-6a0b-3ca5900b4c05",
          "name": "Maria Santos",
          "email": "maria@example.com",
          "pivot": {
            "conversation_id": 1,
            "user_id": 11,
            "unread_count": 0,
            "last_read_at": "2025-10-15T10:30:00.000000Z",
            "joined_at": "2025-10-01T08:00:00.000000Z",
            "created_at": "2025-10-01T08:00:00.000000Z",
            "updated_at": "2025-10-15T10:30:00.000000Z"
          }
        }
      ]
    }
  ]
}
```

---

## Explicação da Estrutura JSON

### `data[]` – Array de Conversas

| Campo               | Tipo      | Descrição |
| ------------------- | --------- | --------- |
| `uuid`              | `uuid`    | Identificador único da conversa. |
| `taggable_type`     | `string`  | Tipo do modelo polimórfico associado à conversa (quando aplicável). |
| `last_message_text` | `string`  | Texto da última mensagem enviada na conversa. |
| `last_message_at`   | `string`  | Data e hora da última mensagem (formato ISO-8601). |
| `is_archived`       | `boolean` | Indica se a conversa está arquivada. |
| `created_at`        | `string`  | Data de criação da conversa (formato ISO-8601). |
| `updated_at`        | `string`  | Data da última atualização da conversa (formato ISO-8601). |
| `taggable_info`     | `object`  | Informações do contexto associado à conversa. |
| `participants[]`    | `array`   | Lista de participantes da conversa. |

### `taggable_info`

| Campo   | Tipo     | Descrição |
| ------- | -------- | --------- |
| `type`  | `string` | Nome da classe do modelo associado. |
| `uuid`  | `uuid`   | UUID do modelo associado. |
| `title` | `string` | Título ou nome do modelo associado. |

### `participants[]` – Participante

| Campo   | Tipo     | Descrição |
| ------- | -------- | --------- |
| `uuid`  | `uuid`   | UUID do participante. |
| `name`  | `string` | Nome do participante. |
| `email` | `string` | E-mail do participante. |
| `pivot` | `object` | Informações da relação muitos-para-muitos. |

### `pivot` – Informações do Participante na Conversa

| Campo             | Tipo      | Descrição |
| ----------------- | --------- | --------- |
| `conversation_id` | `integer` | ID interno da conversa. |
| `user_id`         | `integer` | ID interno do usuário. |
| `unread_count`    | `integer` | Quantidade de mensagens não lidas. |
| `last_read_at`    | `string`  | Data e hora da última leitura (formato ISO-8601). |
| `joined_at`       | `string`  | Data e hora em que o participante entrou na conversa (formato ISO-8601). |
| `created_at`      | `string`  | Data de criação do relacionamento (formato ISO-8601). |
| `updated_at`      | `string`  | Data da última atualização do relacionamento (formato ISO-8601). |

---

## Status HTTP

- 200: Sucesso
- 401: Não autenticado
- 500: Erro interno

---

## Notas

* As conversas são ordenadas pela data da última mensagem (`last_message_at`) em ordem decrescente.
* Apenas conversas nas quais o usuário autenticado é participante são retornadas.
* O campo `taggable_info` pode ser `null` se não houver contexto associado à conversa.
* O contador `unread_count` indica quantas mensagens o participante ainda não leu.

---

## Relacionados

- [Criar Conversa](./ChatConversationsCreate.md)
- [Listar Mensagens de uma Conversa](./ChatConversationMessages.md)

---

## Changelog

- 2025-10-15: Criação inicial da documentação.
