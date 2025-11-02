# Pigeons – Registrar Token de Dispositivo

## Endpoint
```
POST /api/v1/pigeons/chat/devices
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
| `token` | string | Sim | Token do Firebase Cloud Messaging (FCM) do dispositivo (máx 4096 caracteres) | - |
| `platform` | string | Sim | Plataforma do dispositivo | android, ios, web |
| `device.type` | string | Não | Tipo de dispositivo | - |
| `device.os` | string | Não | Nome do sistema operacional | - |
| `device.os_version` | string | Não | Versão do sistema operacional | - |
| `device.app_version` | string | Não | Versão da aplicação | - |
| `device.model` | string | Não | Modelo do dispositivo | - |
| `device.browser` | string | Não | Nome do navegador (para web) | - |
| `device.browser_version` | string | Não | Versão do navegador | - |

## Exemplos

### Requisição
```bash
curl -X POST \
  https://api.example.com/api/v1/pigeons/chat/devices \
  -H 'Authorization: Bearer {token}' \
  -H 'X-PUBLIC-KEY: {platform-key}' \
  -H 'Content-Type: application/json' \
  -H 'Accept-Language: pt-BR' \
  -d '{
    "token": "fGxR8hJ3T...",
    "platform": "android",
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
  "message": "Device token registrado",
  "data": {
    "id": "67890abcdef12345",
    "token": "fGxR8hJ3T...",
    "platform": "android"
  }
}
```

## Estrutura JSON Explicada
| Campo | Tipo | Descrição |
|-------|------|-----------|
| `message` | string | Mensagem de sucesso |
| `data` | object | Informações do token de dispositivo registrado |
| `data.id` | string | ID do documento do token de dispositivo |
| `data.token` | string | Token FCM do dispositivo |
| `data.platform` | string | Plataforma do dispositivo |

## Status HTTP
| Código | Descrição |
|--------|-----------|
| 201 | Criado – token de dispositivo registrado com sucesso |
| 400 | Requisição incorreta – parâmetros inválidos |
| 401 | Não autorizado – token inválido ou ausente |
| 403 | Proibido – permissões insuficientes |
| 422 | Entidade não processável – erros de validação |
| 429 | Muitas requisições – limite de taxa excedido |
| 500 | Erro interno do servidor |

## Erros
```json
{
  "message": "Os dados fornecidos não são válidos.",
  "errors": {
    "token": [
      "O campo token é obrigatório."
    ],
    "platform": [
      "O campo platform é obrigatório."
    ]
  }
}
```

## Notas
- Os tokens de dispositivo são usados para notificações push do Firebase Cloud Messaging (FCM)
- Se já existir um token para a mesma plataforma e usuário, será deletado e recriado com metadados atualizados
- Os tokens são associados ao usuário autenticado e à plataforma atual (via `X-PUBLIC-KEY`)
- O comprimento máximo do token é de 4096 caracteres (limite do token FCM)
- As plataformas suportadas são: `android`, `ios` e `web`
- Os metadados do dispositivo são opcionais, mas recomendados para análise e depuração
- Os tokens são armazenados no MongoDB para consultas eficientes

## Relacionado
- [Deletar Token de Dispositivo](./DevicesDestroy.md)
- [Enviar Mensagem](./MessagesStore.md)
- [Listar Conversas](./ConversationsIndex.md)
