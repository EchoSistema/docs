# Chat – Enviar Mensagem

## Endpoint

`POST /api/v1/chat/messages`

Envia uma nova mensagem em uma conversa existente, com suporte a diferentes tipos de conteúdo, anexos e informações do dispositivo.

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

| Parâmetro               | Tipo     | Obrigatório | Descrição |
| ----------------------- | -------- | ----------- | --------- |
| `conversation_uuid`     | `uuid`   | Sim         | UUID da conversa. |
| `receiver_uuid`         | `uuid`   | Sim         | UUID do destinatário da mensagem. |
| `context_text`          | `string` | Sim         | Texto da mensagem (máx. 5000 caracteres). |
| `context_type`          | `string` | Sim         | Tipo de conteúdo: `text`, `image`, `file`, `audio`, `video`. |
| `attachments[]`         | `array`  | Não         | Lista de anexos (estrutura livre). |
| `metadata`              | `object` | Não         | Metadados adicionais (estrutura livre). |
| `device.type`           | `string` | Sim         | Tipo de dispositivo: `mobile`, `web`, `desktop`. |
| `device.os`             | `string` | Sim         | Sistema operacional (ex.: `Windows`, `iOS`, `Android`). |
| `device.os_version`     | `string` | Não         | Versão do sistema operacional. |
| `device.app_version`    | `string` | Não         | Versão da aplicação. |
| `device.model`          | `string` | Não         | Modelo do dispositivo. |
| `device.browser`        | `string` | Não         | Nome do navegador. |
| `device.browser_version`| `string` | Não         | Versão do navegador. |

---

## Exemplos

### Exemplo de Requisição (curl)

```bash
curl -X POST \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <chave>" \
  -H "Content-Type: application/json" \
  -d '{
    "conversation_uuid": "9b732d6a-2e56-4db8-9b2e-6fd8c14e7f28",
    "receiver_uuid": "6b420a39-0b23-3a95-6a0b-3ca5900b4c05",
    "context_text": "Olá, tudo bem?",
    "context_type": "text",
    "attachments": [],
    "metadata": {
      "source": "web_app"
    },
    "device": {
      "type": "web",
      "os": "Windows",
      "os_version": "10",
      "app_version": "1.0.0",
      "browser": "Chrome",
      "browser_version": "120.0"
    }
  }' \
  "https://sandbox.seu-dominio.com/api/v1/chat/messages"
```

### Exemplo de Corpo da Requisição

```json
{
  "conversation_uuid": "9b732d6a-2e56-4db8-9b2e-6fd8c14e7f28",
  "receiver_uuid": "6b420a39-0b23-3a95-6a0b-3ca5900b4c05",
  "context_text": "Olá, tudo bem?",
  "context_type": "text",
  "attachments": [],
  "metadata": {
    "source": "web_app"
  },
  "device": {
    "type": "web",
    "os": "Windows",
    "os_version": "10",
    "app_version": "1.0.0",
    "browser": "Chrome",
    "browser_version": "120.0"
  }
}
```

### Exemplo de Resposta

```json
{
  "message": "Mensagem enviada com sucesso",
  "data": {
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
    "metadata": {
      "source": "web_app"
    },
    "reactions": [],
    "status": {
      "is_read": false,
      "read_at": null,
      "delivered_at": "2025-10-15T10:30:00+00:00",
      "is_deleted": false
    },
    "timestamps": {
      "created_at": "2025-10-15T10:30:00+00:00",
      "updated_at": "2025-10-15T10:30:00+00:00"
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
}
```

---

## Explicação da Estrutura JSON

### Resposta de Sucesso

| Campo     | Tipo     | Descrição |
| --------- | -------- | --------- |
| `message` | `string` | Mensagem de confirmação. |
| `data`    | `object` | Objeto da mensagem criada. |

### `data` – Objeto Mensagem

Ver estrutura completa em [Listar Mensagens de uma Conversa](./ChatConversationMessages.md).

---

## Status HTTP

- 201: Criado com sucesso
- 400: Requisição inválida
- 401: Não autenticado
- 403: Proibido (usuário não é participante da conversa)
- 404: Conversa ou destinatário não encontrado
- 422: Erro de validação
- 500: Erro interno

---

## Erros

### Exemplo de Erro de Validação

```json
{
  "message": "The conversation uuid field is required.",
  "errors": {
    "conversation_uuid": [
      "The conversation uuid field is required."
    ],
    "context_text": [
      "The context text must not be greater than 5000 characters."
    ]
  }
}
```

### Exemplo de Erro Interno

```json
{
  "success": false,
  "message": "Failed to send message",
  "error": "Database connection error"
}
```

---

## Notas

* O usuário autenticado deve ser participante da conversa especificada.
* O texto da mensagem é limitado a 5000 caracteres.
* A mensagem é criada com status `is_read: false` e `read_at: null` por padrão.
* O campo `delivered_at` é preenchido automaticamente com a data/hora atual.
* A conversa é atualizada com `last_message_text` e `last_message_at` após o envio.
* Um evento `MessageSent` é disparado via broadcast para notificar outros participantes em tempo real.
* Os campos de dispositivo (`device`) são armazenados para análise e estatísticas.

---

## Relacionados

- [Listar Conversas](./ChatConversationsList.md)
- [Criar Conversa](./ChatConversationsCreate.md)
- [Listar Mensagens de uma Conversa](./ChatConversationMessages.md)
- [Marcar Mensagem como Lida](./ChatMessageMarkRead.md)
- [Estatísticas de Dispositivos](./ChatDeviceStats.md)

---

## Changelog

- 2025-10-15: Criação inicial da documentação.
