# Chat – Criar Conversa

## Endpoint

`POST /api/v1/chat/conversations`

Cria uma nova conversa entre o usuário autenticado e outro participante.

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

### Parâmetros do Corpo da Requisição

| Parâmetro          | Tipo      | Obrigatório | Descrição |
| ------------------ | --------- | ----------- | --------- |
| `participant_uuid` | `uuid`    | Sim         | UUID do participante que será adicionado à conversa. |
| `taggable_type`    | `string`  | Não         | Tipo do modelo polimórfico a ser associado à conversa (ex.: `App\Models\Order`). |
| `taggable_id`      | `integer` | Não         | ID do modelo polimórfico a ser associado à conversa. |

---

## Exemplos

### Exemplo de Requisição (curl)

```bash
curl -X POST \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <chave>" \
  -H "Content-Type: application/json" \
  -d '{
    "participant_uuid": "6b420a39-0b23-3a95-6a0b-3ca5900b4c05",
    "taggable_type": "App\\Models\\Order",
    "taggable_id": 123
  }' \
  "https://sandbox.seu-dominio.com/api/v1/chat/conversations"
```

### Exemplo de Corpo da Requisição

```json
{
  "participant_uuid": "6b420a39-0b23-3a95-6a0b-3ca5900b4c05",
  "taggable_type": "App\\Models\\Order",
  "taggable_id": 123
}
```

### Exemplo de Resposta

```json
{
  "message": "Conversa criada com sucesso",
  "data": {
    "uuid": "9b732d6a-2e56-4db8-9b2e-6fd8c14e7f28",
    "taggable_type": "App\\Models\\Order",
    "taggable_id": 123,
    "last_message_text": null,
    "last_message_at": null,
    "is_archived": false,
    "created_at": "2025-10-15T10:30:00.000000Z",
    "updated_at": "2025-10-15T10:30:00.000000Z",
    "participants": [
      {
        "uuid": "7c531b4a-1c34-4ba6-7b0c-4db6a12c5d16",
        "name": "João Silva",
        "email": "joao@example.com",
        "pivot": {
          "conversation_id": 1,
          "user_id": 10,
          "unread_count": 0,
          "last_read_at": null,
          "joined_at": "2025-10-15T10:30:00.000000Z",
          "created_at": "2025-10-15T10:30:00.000000Z",
          "updated_at": "2025-10-15T10:30:00.000000Z"
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
          "last_read_at": null,
          "joined_at": "2025-10-15T10:30:00.000000Z",
          "created_at": "2025-10-15T10:30:00.000000Z",
          "updated_at": "2025-10-15T10:30:00.000000Z"
        }
      }
    ]
  }
}
```

---

## Explicação da Estrutura JSON

### Resposta de Sucesso

| Campo     | Tipo     | Descrição |
| --------- | -------- | --------- |
| `message` | `string` | Mensagem de confirmação. |
| `data`    | `object` | Objeto da conversa criada com participantes carregados. |

### `data` – Objeto Conversa

| Campo               | Tipo      | Descrição |
| ------------------- | --------- | --------- |
| `uuid`              | `uuid`    | Identificador único da conversa. |
| `taggable_type`     | `string`  | Tipo do modelo polimórfico associado (quando aplicável). |
| `taggable_id`       | `integer` | ID do modelo polimórfico associado (quando aplicável). |
| `last_message_text` | `string`  | Texto da última mensagem (null em conversas novas). |
| `last_message_at`   | `string`  | Data da última mensagem (null em conversas novas). |
| `is_archived`       | `boolean` | Indica se a conversa está arquivada. |
| `created_at`        | `string`  | Data de criação da conversa (formato ISO-8601). |
| `updated_at`        | `string`  | Data da última atualização (formato ISO-8601). |
| `participants[]`    | `array`   | Lista de participantes da conversa. |

---

## Status HTTP

- 201: Criado com sucesso
- 400: Requisição inválida
- 401: Não autenticado
- 422: Erro de validação (participante não encontrado ou parâmetros inválidos)
- 500: Erro interno

---

## Erros

### Exemplo de Erro de Validação

```json
{
  "message": "The participant uuid field is required.",
  "errors": {
    "participant_uuid": [
      "The participant uuid field is required."
    ]
  }
}
```

### Exemplo de Erro Interno

```json
{
  "success": false,
  "message": "Failed to create conversation",
  "error": "Database connection error"
}
```

---

## Notas

* A conversa é criada com ambos os participantes (usuário autenticado e participante especificado).
* Os campos `taggable_type` e `taggable_id` são opcionais e permitem associar a conversa a outro modelo (ex.: pedido, reserva, etc.).
* A conversa é criada com `is_archived` definido como `false` por padrão.
* Ambos os participantes têm `unread_count` inicializado em `0`.
* A data de entrada (`joined_at`) de ambos os participantes é definida no momento da criação.

---

## Relacionados

- [Listar Conversas](./ChatConversationsList.md)
- [Enviar Mensagem](./ChatMessageSend.md)
- [Listar Mensagens de uma Conversa](./ChatConversationMessages.md)

---

## Changelog

- 2025-10-15: Criação inicial da documentação.
