# Pigeons – Adicionar Reação à Mensagem

## Endpoint
```
POST /api/v1/pigeons/chat/messages/{messageId}/reactions
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

### Parâmetros de Rota
| Parâmetro | Tipo | Obrigatório | Descrição |
|-----------|------|-------------|-----------|
| `messageId` | string | Sim | ID da mensagem |

### Corpo da Requisição
| Parâmetro | Tipo | Obrigatório | Descrição |
|-----------|------|-------------|-----------|
| `emoji` | string | Sim | Reação emoji (máx 10 caracteres) |

## Exemplos

### Requisição
```bash
curl -X POST \
  https://api.example.com/api/v1/pigeons/chat/messages/msg_123456789/reactions \
  -H 'Authorization: Bearer {token}' \
  -H 'X-PUBLIC-KEY: {platform-key}' \
  -H 'Content-Type: application/json' \
  -H 'Accept-Language: pt-BR' \
  -d '{
    "emoji": "👍"
  }'
```

### Resposta
```json
{
  "message": "Reação adicionada com sucesso"
}
```

## Estrutura JSON Explicada
| Campo | Tipo | Descrição |
|-------|------|-----------|
| `message` | string | Mensagem de sucesso |

## Status HTTP
| Código | Descrição |
|--------|-----------|
| 200 | Sucesso – reação adicionada |
| 400 | Requisição incorreta – parâmetros inválidos |
| 401 | Não autorizado – token inválido ou ausente |
| 403 | Proibido – permissões insuficientes |
| 404 | Não encontrado – a mensagem não existe |
| 422 | Entidade não processável – erros de validação |
| 429 | Muitas requisições – limite de taxa excedido |
| 500 | Erro interno do servidor |

## Erros
```json
{
  "message": "Os dados fornecidos não são válidos.",
  "errors": {
    "emoji": [
      "O campo emoji é obrigatório."
    ]
  }
}
```

```json
{
  "message": "Mensagem não encontrada"
}
```

## Notas
- Qualquer participante da conversa pode adicionar reações às mensagens
- O emoji está limitado a 10 caracteres (suporta a maioria dos emojis individuais e sequências)
- Cada usuário pode adicionar múltiplas reações diferentes à mesma mensagem
- Adicionar o mesmo emoji novamente não criará uma duplicata
- As reações são armazenadas no array `reactions` da mensagem com o UUID do usuário e o timestamp
- Atualizações em tempo real podem ser transmitidas para notificar os participantes de novas reações

## Relacionado
- [Obter Mensagens da Conversa](./ConversationMessagesIndex.md)
- [Enviar Mensagem](./MessagesStore.md)
- [Marcar Mensagem como Lida](./MessagesRead.md)
