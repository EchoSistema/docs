# Pigeons – Obter Mensagens da Conversa

## Endpoint
```
GET /api/v1/pigeons/chat/conversations/{uuid}/messages
```

## Autenticação
Obrigatória – Bearer token com habilidade `backoffice`.

## Cabeçalhos
| Cabeçalho | Tipo | Obrigatório | Descrição |
|-----------|------|-------------|-----------|
| `Authorization` | string | Sim | Bearer {token} |
| `X-PUBLIC-KEY` | string | Sim | Chave pública da plataforma |
| `Accept-Language` | string | Não | Locale para campos traduzíveis (pt-BR, en, es) |

## Parâmetros

### Parâmetros de Rota
| Parâmetro | Tipo | Obrigatório | Descrição |
|-----------|------|-------------|-----------|
| `uuid` | string | Sim | UUID da conversa |

## Exemplos

### Requisição
```bash
curl -X GET \
  https://api.example.com/api/v1/pigeons/chat/conversations/550e8400-e29b-41d4-a716-446655440000/messages \
  -H 'Authorization: Bearer {token}' \
  -H 'X-PUBLIC-KEY: {platform-key}' \
  -H 'Accept-Language: pt-BR'
```

### Resposta
```json
{
  "data": [
    {
      "message_id": "msg_123456789",
      "id": "msg_123456789",
      "conversation_uuid": "550e8400-e29b-41d4-a716-446655440000",
      "sender": {
        "uuid": "660e8400-e29b-41d4-a716-446655440001",
        "name": "João Silva",
        "avatar": "https://example.com/avatars/joao.jpg"
      },
      "receiver": {
        "uuid": "770e8400-e29b-41d4-a716-446655440002",
        "name": "Usuário Atual",
        "avatar": "https://example.com/avatars/usuario.jpg"
      },
      "context": {
        "text": "Olá, como você está?",
        "type": "text"
      },
      "attachments": [],
      "metadata": {},
      "reactions": [
        {
          "user_uuid": "770e8400-e29b-41d4-a716-446655440002",
          "emoji": "👍",
          "created_at": "2025-11-02T10:35:00+00:00"
        }
      ],
      "status": {
        "sent": true,
        "delivered": true,
        "read": true
      },
      "timestamps": {
        "sent_at": "2025-11-02T10:30:00+00:00",
        "delivered_at": "2025-11-02T10:30:05+00:00",
        "read_at": "2025-11-02T10:31:00+00:00"
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
  ],
  "conversation": {
    "uuid": "550e8400-e29b-41d4-a716-446655440000",
    "taggable_type": "App\\Models\\Article",
    "last_message_text": "Olá, como você está?",
    "last_message_at": "2025-11-02T10:30:00+00:00",
    "is_archived": false,
    "created_at": "2025-11-01T08:00:00+00:00",
    "updated_at": "2025-11-02T10:30:00+00:00",
    "taggable_info": {
      "id": 123,
      "title": "Título do Artigo"
    }
  }
}
```

## Estrutura JSON Explicada
| Campo | Tipo | Descrição |
|-------|------|-----------|
| `data` | array | Array de objetos de mensagem |
| `data[].message_id` | string | Identificador único da mensagem |
| `data[].conversation_uuid` | string | UUID da conversa |
| `data[].sender` | object | Informações do usuário remetente |
| `data[].receiver` | object | Informações do usuário destinatário |
| `data[].context` | object | Conteúdo e tipo da mensagem |
| `data[].context.text` | string | Conteúdo de texto da mensagem |
| `data[].context.type` | string | Tipo de mensagem (text, image, file, audio, video) |
| `data[].attachments` | array | Array de arquivos anexados |
| `data[].metadata` | object | Metadados adicionais da mensagem |
| `data[].reactions` | array | Array de reações emoji |
| `data[].status` | object | Status de entrega da mensagem |
| `data[].timestamps` | object | Timestamps da mensagem (enviado, entregue, lido) |
| `data[].pbk` | string | Chave pública da plataforma |
| `data[].device` | object | Informações do dispositivo de onde a mensagem foi enviada |
| `conversation` | object | Metadados da conversa |

## Status HTTP
| Código | Descrição |
|--------|-----------|
| 200 | Sucesso – mensagens recuperadas |
| 401 | Não autorizado – token inválido ou ausente |
| 403 | Proibido – usuário não é participante desta conversa |
| 404 | Não encontrado – a conversa não existe |
| 429 | Muitas requisições – limite de taxa excedido |
| 500 | Erro interno do servidor |

## Erros
```json
{
  "message": "Conversa não encontrada"
}
```

## Notas
- Apenas participantes da conversa podem recuperar mensagens
- As mensagens são armazenadas no MongoDB para alto desempenho
- A resposta inclui tanto mensagens quanto metadados da conversa
- As mensagens incluem informações do dispositivo para rastrear de qual dispositivo cada mensagem foi enviada
- As reações são armazenadas como um array e podem ser adicionadas por qualquer participante

## Relacionado
- [Listar Conversas](./ConversationsIndex.md)
- [Enviar Mensagem](./MessagesStore.md)
- [Marcar Mensagem como Lida](./MessagesRead.md)
- [Adicionar Reação](./MessagesReactionsStore.md)
