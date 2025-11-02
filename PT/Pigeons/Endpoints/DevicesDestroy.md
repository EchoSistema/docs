# Pigeons – Deletar Token de Dispositivo

## Endpoint
```
DELETE /api/v1/pigeons/chat/devices/{token}
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
| `token` | string | Sim | Token FCM do dispositivo a deletar |

## Exemplos

### Requisição
```bash
curl -X DELETE \
  https://api.example.com/api/v1/pigeons/chat/devices/fGxR8hJ3T... \
  -H 'Authorization: Bearer {token}' \
  -H 'X-PUBLIC-KEY: {platform-key}' \
  -H 'Accept-Language: pt-BR'
```

### Resposta
```json
{
  "message": "Device token removido"
}
```

## Estrutura JSON Explicada
| Campo | Tipo | Descrição |
|-------|------|-----------|
| `message` | string | Mensagem de sucesso |

## Status HTTP
| Código | Descrição |
|--------|-----------|
| 200 | Sucesso – token de dispositivo deletado |
| 401 | Não autorizado – token inválido ou ausente |
| 403 | Proibido – permissões insuficientes |
| 429 | Muitas requisições – limite de taxa excedido |
| 500 | Erro interno do servidor |

## Notas
- Deleta o token de dispositivo para o usuário autenticado e a plataforma atual
- Apenas o proprietário do token pode deletar seus próprios tokens
- Se o token não existir, a operação é concluída com sucesso (idempotente)
- Após a deleção, notificações push não serão mais enviadas para este dispositivo
- Os usuários devem chamar este endpoint quando:
  - Fazem logout da aplicação
  - Desinstalam a aplicação
  - Desabilitam notificações push
- O token é comparado com o UUID do usuário e a chave pública da plataforma por segurança

## Relacionado
- [Registrar Token de Dispositivo](./DevicesStore.md)
- [Enviar Mensagem](./MessagesStore.md)
- [Listar Conversas](./ConversationsIndex.md)
