# Pigeons – Marcar Mensagem como Lida

## Endpoint
```
PATCH /api/v1/pigeons/chat/messages/{messageId}/read
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
| `messageId` | string | Sim | ID da mensagem |

## Exemplos

### Requisição
```bash
curl -X PATCH \
  https://api.example.com/api/v1/pigeons/chat/messages/msg_123456789/read \
  -H 'Authorization: Bearer {token}' \
  -H 'X-PUBLIC-KEY: {platform-key}' \
  -H 'Accept-Language: pt-BR'
```

### Resposta
```json
{
  "message": "Mensagem marcada como lida"
}
```

## Estrutura JSON Explicada
| Campo | Tipo | Descrição |
|-------|------|-----------|
| `message` | string | Mensagem de sucesso |

## Status HTTP
| Código | Descrição |
|--------|-----------|
| 200 | Sucesso – mensagem marcada como lida |
| 401 | Não autorizado – token inválido ou ausente |
| 403 | Proibido – usuário não é o destinatário desta mensagem |
| 404 | Não encontrado – a mensagem não existe |
| 429 | Muitas requisições – limite de taxa excedido |
| 500 | Erro interno do servidor |

## Erros
```json
{
  "message": "Mensagem não encontrada"
}
```

```json
{
  "message": "Não autorizado"
}
```

## Notas
- Apenas o destinatário da mensagem pode marcá-la como lida
- O `status.read` da mensagem é definido como `true`
- O `timestamps.read_at` é definido com o timestamp atual
- Um evento de confirmação de leitura em tempo real é transmitido para o canal da conversa
- O `unread_count` do destinatário para esta conversa é decrementado
- Se a mensagem já estava marcada como lida, a operação é idempotente

## Relacionado
- [Obter Mensagens da Conversa](./ConversationMessagesIndex.md)
- [Enviar Mensagem](./MessagesStore.md)
- [Listar Conversas](./ConversationsIndex.md)
