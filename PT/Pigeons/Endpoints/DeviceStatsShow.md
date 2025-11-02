# Pigeons – Obter Estatísticas de Dispositivos

## Endpoint
```
GET /api/v1/pigeons/chat/stats/devices
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
  https://api.example.com/api/v1/pigeons/chat/stats/devices \
  -H 'Authorization: Bearer {token}' \
  -H 'X-PUBLIC-KEY: {platform-key}' \
  -H 'Accept-Language: pt-BR'
```

### Resposta
```json
{
  "data": [
    {
      "_id": "mobile",
      "count": 150
    },
    {
      "_id": "web",
      "count": 85
    },
    {
      "_id": "desktop",
      "count": 23
    }
  ]
}
```

## Estrutura JSON Explicada
| Campo | Tipo | Descrição |
|-------|------|-----------|
| `data` | array | Array de estatísticas por tipo de dispositivo |
| `data[]._id` | string | Tipo de dispositivo (mobile, web, desktop) |
| `data[].count` | integer | Número de mensagens enviadas deste tipo de dispositivo |

## Status HTTP
| Código | Descrição |
|--------|-----------|
| 200 | Sucesso – estatísticas recuperadas |
| 401 | Não autorizado – token inválido ou ausente |
| 403 | Proibido – permissões insuficientes |
| 429 | Muitas requisições – limite de taxa excedido |
| 500 | Erro interno do servidor |

## Notas
- Retorna estatísticas agregadas de mensagens por tipo de dispositivo
- Inclui apenas estatísticas de conversas onde o usuário autenticado é participante
- Os dados são agregados do MongoDB usando o pipeline de agregação `$group`
- Se o usuário não tiver conversas, retorna um array vazio
- Os tipos de dispositivo são: `mobile`, `web` ou `desktop`
- A contagem representa o número total de mensagens enviadas de cada tipo de dispositivo em todas as conversas do usuário

## Relacionado
- [Listar Conversas](./ConversationsIndex.md)
- [Obter Mensagens da Conversa](./ConversationMessagesIndex.md)
- [Enviar Mensagem](./MessagesStore.md)
