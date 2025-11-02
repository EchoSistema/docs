# Pigeons – Enviar Mensagem

## Endpoint
```
POST /api/v1/pigeons/chat/messages
```

## Autenticação
Obrigatória – Bearer token com habilidade `backoffice`.

## Cabeçalhos
| Cabeçalho | Tipo | Obrigatório | Descrição |
|-----------|------|-------------|-----------|
| `Authorization` | string | Sim | Bearer {token} |
| `X-PUBLIC-KEY` | string | Sim | Chave pública da plataforma |
| `Accept-Language` | string | Não | Locale para campos traduzíveis (pt-BR, en, es) |
| `Content-Type` | string | Sim | application/json |

## Parâmetros

### Corpo da Requisição
| Parâmetro | Tipo | Obrigatório | Descrição | Valores |
|-----------|------|-------------|-----------|---------|
| `conversation_uuid` | string | Sim | UUID da conversa | UUID válido de conversa |
| `receiver_uuid` | string | Sim | UUID do destinatário da mensagem | UUID válido de usuário |
| `context_text` | string | Sim | Conteúdo de texto da mensagem (máx 5000 caracteres) | - |
| `context_type` | string | Sim | Tipo de mensagem | text, image, file, audio, video |
| `attachments` | array | Não | Array de arquivos anexados | - |
| `metadata` | object | Não | Metadados adicionais da mensagem | - |
| `device.type` | string | Sim | Tipo de dispositivo | mobile, web, desktop |
| `device.os` | string | Sim | Nome do sistema operacional | - |
| `device.os_version` | string | Não | Versão do sistema operacional | - |
| `device.app_version` | string | Não | Versão da aplicação | - |
| `device.model` | string | Não | Modelo do dispositivo | - |
| `device.browser` | string | Não | Nome do navegador (para web/desktop) | - |
| `device.browser_version` | string | Não | Versão do navegador | - |

## Exemplos

### Requisição
```bash
curl -X POST \
  https://api.example.com/api/v1/pigeons/chat/messages \
  -H 'Authorization: Bearer {token}' \
  -H 'X-PUBLIC-KEY: {platform-key}' \
  -H 'Content-Type: application/json' \
  -H 'Accept-Language: pt-BR' \
  -d '{
    "conversation_uuid": "550e8400-e29b-41d4-a716-446655440000",
    "receiver_uuid": "660e8400-e29b-41d4-a716-446655440001",
    "context_text": "Olá, como você está?",
    "context_type": "text",
    "attachments": [],
    "metadata": {},
    "device": {
      "type": "mobile",
      "os": "Android",
      "os_version": "14",
      "app_version": "1.0.0",
      "model": "Samsung Galaxy S23"
    }
  }'
```

### Resposta
```json
{
  "message": "Mensagem enviada com sucesso",
  "data": {
    "message_id": "msg_123456789",
    "conversation_uuid": "550e8400-e29b-41d4-a716-446655440000",
    "sender": {
      "uuid": "770e8400-e29b-41d4-a716-446655440002",
      "name": "Usuário Atual",
      "avatar": "https://example.com/avatars/usuario.jpg"
    },
    "receiver": {
      "uuid": "660e8400-e29b-41d4-a716-446655440001",
      "name": "João Silva",
      "avatar": "https://example.com/avatars/joao.jpg"
    },
    "context": {
      "text": "Olá, como você está?",
      "type": "text"
    },
    "attachments": [],
    "metadata": {},
    "reactions": [],
    "status": {
      "sent": true,
      "delivered": false,
      "read": false
    },
    "timestamps": {
      "sent_at": "2025-11-02T10:30:00+00:00"
    },
    "pbk": "platform_public_key",
    "device": {
      "type": "mobile",
      "os": "Android",
      "os_version": "14",
      "app_version": "1.0.0",
      "model": "Samsung Galaxy S23"
    }
  }
}
```

## Estrutura JSON Explicada
| Campo | Tipo | Descrição |
|-------|------|-----------|
| `message` | string | Mensagem de sucesso |
| `data` | object | Objeto da mensagem criada |
| `data.message_id` | string | Identificador único da mensagem |
| `data.conversation_uuid` | string | UUID da conversa |
| `data.sender` | object | Informações do usuário remetente (usuário autenticado) |
| `data.receiver` | object | Informações do usuário destinatário |
| `data.context` | object | Conteúdo e tipo da mensagem |
| `data.attachments` | array | Array de arquivos anexados |
| `data.metadata` | object | Metadados adicionais da mensagem |
| `data.reactions` | array | Array de reações emoji (vazio para novas mensagens) |
| `data.status` | object | Status de entrega da mensagem |
| `data.timestamps` | object | Timestamps da mensagem |
| `data.pbk` | string | Chave pública da plataforma |
| `data.device` | object | Informações do dispositivo |

## Status HTTP
| Código | Descrição |
|--------|-----------|
| 201 | Criado – mensagem enviada com sucesso |
| 400 | Requisição incorreta – parâmetros inválidos |
| 401 | Não autorizado – token inválido ou ausente |
| 403 | Proibido – usuário não é participante desta conversa |
| 404 | Não encontrado – conversa ou destinatário não encontrado |
| 422 | Entidade não processável – erros de validação |
| 429 | Muitas requisições – limite de taxa excedido |
| 500 | Erro interno do servidor |

## Erros
```json
{
  "message": "Os dados fornecidos não são válidos.",
  "errors": {
    "context_text": [
      "O campo context text é obrigatório."
    ],
    "device.type": [
      "O campo device.type é obrigatório."
    ]
  }
}
```

## Notas
- As mensagens são armazenadas no MongoDB para alto desempenho
- Notificações em tempo real são enviadas via WebSockets/broadcasting
- Notificações push são enviadas para os tokens de dispositivo registrados
- `last_message_at` e `last_message_text` da conversa são atualizados automaticamente
- O `unread_count` do participante é incrementado automaticamente para o destinatário
- O comprimento máximo da mensagem é de 5000 caracteres
- Informações do dispositivo são necessárias para rastrear as fontes das mensagens

## Relacionado
- [Listar Conversas](./ConversationsIndex.md)
- [Obter Mensagens da Conversa](./ConversationMessagesIndex.md)
- [Marcar Mensagem como Lida](./MessagesRead.md)
- [Adicionar Reação](./MessagesReactionsStore.md)
