# Pigeons – Criar Conversa

## Endpoint
```
POST /api/v1/pigeons/chat/conversations
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
| Parâmetro | Tipo | Obrigatório | Descrição |
|-----------|------|-------------|-----------|
| `participant_uuid` | string | Sim | UUID do usuário para iniciar a conversa |
| `taggable_type` | string | Não | Tipo de entidade relacionada (ex., "App\\Models\\Article") |
| `taggable_id` | integer | Não | ID da entidade relacionada |

## Exemplos

### Requisição
```bash
curl -X POST \
  https://api.example.com/api/v1/pigeons/chat/conversations \
  -H 'Authorization: Bearer {token}' \
  -H 'X-PUBLIC-KEY: {platform-key}' \
  -H 'Content-Type: application/json' \
  -H 'Accept-Language: pt-BR' \
  -d '{
    "participant_uuid": "660e8400-e29b-41d4-a716-446655440001",
    "taggable_type": "App\\Models\\Article",
    "taggable_id": 123
  }'
```

### Resposta
```json
{
  "message": "Conversa criada com sucesso",
  "data": {
    "uuid": "550e8400-e29b-41d4-a716-446655440000",
    "taggable_type": "App\\Models\\Article",
    "taggable_id": 123,
    "last_message_text": null,
    "last_message_at": null,
    "is_archived": false,
    "created_at": "2025-11-02T10:30:00+00:00",
    "updated_at": "2025-11-02T10:30:00+00:00",
    "participants": [
      {
        "uuid": "770e8400-e29b-41d4-a716-446655440002",
        "name": "Usuário Atual",
        "email": "usuario@example.com"
      },
      {
        "uuid": "660e8400-e29b-41d4-a716-446655440001",
        "name": "João Silva",
        "email": "joao@example.com"
      }
    ]
  }
}
```

## Estrutura JSON Explicada
| Campo | Tipo | Descrição |
|-------|------|-----------|
| `message` | string | Mensagem de sucesso |
| `data` | object | Objeto da conversa criada |
| `data.uuid` | string | Identificador único da conversa |
| `data.taggable_type` | string | Tipo de entidade relacionada |
| `data.taggable_id` | integer | ID da entidade relacionada |
| `data.last_message_text` | string\|null | Texto da última mensagem (null para novas conversas) |
| `data.last_message_at` | string\|null | Timestamp ISO 8601 da última mensagem |
| `data.is_archived` | boolean | Status de arquivamento da conversa |
| `data.created_at` | string | Timestamp ISO 8601 de criação |
| `data.updated_at` | string | Timestamp ISO 8601 de atualização |
| `data.participants` | array | Array de usuários participantes |

## Status HTTP
| Código | Descrição |
|--------|-----------|
| 201 | Criado – conversa criada com sucesso |
| 400 | Requisição incorreta – parâmetros inválidos |
| 401 | Não autorizado – token inválido ou ausente |
| 403 | Proibido – permissões insuficientes |
| 422 | Entidade não processável – erros de validação |
| 429 | Muitas requisições – limite de taxa excedido |
| 500 | Erro interno do servidor |

## Errores
```json
{
  "message": "Os dados fornecidos não são válidos.",
  "errors": {
    "participant_uuid": [
      "O campo participant uuid é obrigatório."
    ]
  }
}
```

## Notas
- Se já existir uma conversa entre os dois usuários para o mesmo `taggable_type` e `taggable_id`, a conversa existente pode ser retornada em vez de criar uma duplicata
- O usuário autenticado é automaticamente adicionado como participante
- `taggable_type` e `taggable_id` são opcionais e permitem associar a conversa com uma entidade específica
- `participant_uuid` deve ser um UUID de usuário válido e existente

## Relacionado
- [Listar Conversas](./ConversationsIndex.md)
- [Obter Mensagens da Conversa](./ConversationMessagesIndex.md)
- [Enviar Mensagem](./MessagesStore.md)
