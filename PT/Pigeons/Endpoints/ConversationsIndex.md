# Pigeons – Listar Conversas

## Endpoint
```
GET /api/v1/pigeons/chat/conversations
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
Nenhum parâmetro necessário.

## Exemplos

### Requisição
```bash
curl -X GET \
  https://api.example.com/api/v1/pigeons/chat/conversations \
  -H 'Authorization: Bearer {token}' \
  -H 'X-PUBLIC-KEY: {platform-key}' \
  -H 'Accept-Language: pt-BR'
```

### Resposta
```json
{
  "data": [
    {
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
      },
      "participants": [
        {
          "uuid": "660e8400-e29b-41d4-a716-446655440001",
          "name": "João Silva",
          "email": "joao@example.com",
          "avatar": "https://example.com/avatars/joao.jpg",
          "avatar_url": "https://example.com/avatars/joao.jpg",
          "pivot": {
            "unread_count": 3,
            "last_read_at": "2025-11-02T09:00:00+00:00",
            "joined_at": "2025-11-01T08:00:00+00:00"
          }
        }
      ]
    }
  ]
}
```

## Estrutura JSON Explicada
| Campo | Tipo | Descrição |
|-------|------|-----------|
| `data` | array | Array de objetos de conversa |
| `data[].uuid` | string | Identificador único da conversa |
| `data[].taggable_type` | string | Tipo de entidade relacionada (polimórfico) |
| `data[].last_message_text` | string | Texto da última mensagem |
| `data[].last_message_at` | string | Timestamp ISO 8601 da última mensagem |
| `data[].is_archived` | boolean | Status de arquivamento da conversa |
| `data[].created_at` | string | Timestamp ISO 8601 de criação |
| `data[].updated_at` | string | Timestamp ISO 8601 de atualização |
| `data[].taggable_info` | object | Informações sobre a entidade relacionada |
| `data[].participants` | array | Array de usuários participantes |
| `data[].participants[].uuid` | string | Identificador único do usuário |
| `data[].participants[].name` | string | Nome completo do usuário |
| `data[].participants[].email` | string | Endereço de e-mail do usuário |
| `data[].participants[].avatar` | string | URL do avatar do usuário |
| `data[].participants[].pivot.unread_count` | integer | Número de mensagens não lidas para este participante |
| `data[].participants[].pivot.last_read_at` | string | Timestamp ISO 8601 da última leitura |
| `data[].participants[].pivot.joined_at` | string | Timestamp ISO 8601 de quando o usuário entrou na conversa |

## Status HTTP
| Código | Descrição |
|--------|-----------|
| 200 | Sucesso – conversas recuperadas |
| 401 | Não autorizado – token inválido ou ausente |
| 403 | Proibido – permissões insuficientes |
| 429 | Muitas requisições – limite de taxa excedido |
| 500 | Erro interno do servidor |

## Notas
- As conversas são ordenadas por `last_message_at` decrescente (mais recente primeiro)
- Retorna apenas conversas onde o usuário autenticado é participante
- Inclui participantes com suas imagens de avatar e contagens de mensagens não lidas
- `taggable_type` e `taggable_id` permitem associar conversas com outras entidades (artigos, propriedades, etc.)

## Relacionado
- [Criar Conversa](./ConversationsStore.md)
- [Obter Mensagens da Conversa](./ConversationMessagesIndex.md)
- [Enviar Mensagem](./MessagesStore.md)
