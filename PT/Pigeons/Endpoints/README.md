# Endpoints da API de Chat Pigeons

Esta seção documenta todos os endpoints disponíveis para o sistema de chat Pigeons, que fornece mensagens em tempo real, notificações push e gerenciamento de dispositivos.

## Gerenciamento de Conversas

| Endpoint | Método | Descrição |
|----------|--------|-----------|
| [Listar Conversas](./ConversationsIndex.md) | GET | Recuperar todas as conversas do usuário autenticado |
| [Criar Conversa](./ConversationsStore.md) | POST | Criar uma nova conversa com outro usuário |
| [Obter Mensagens da Conversa](./ConversationMessagesIndex.md) | GET | Recuperar todas as mensagens de uma conversa específica |

## Operações de Mensagens

| Endpoint | Método | Descrição |
|----------|--------|-----------|
| [Enviar Mensagem](./MessagesStore.md) | POST | Enviar uma nova mensagem em uma conversa |
| [Marcar Mensagem como Lida](./MessagesRead.md) | PATCH | Marcar uma mensagem como lida pelo usuário autenticado |
| [Adicionar Reação](./MessagesReactionsStore.md) | POST | Adicionar uma reação emoji a uma mensagem |

## Gerenciamento de Dispositivos

| Endpoint | Método | Descrição |
|----------|--------|-----------|
| [Registrar Token de Dispositivo](./DevicesStore.md) | POST | Registrar um token de dispositivo FCM para notificações push |
| [Deletar Token de Dispositivo](./DevicesDestroy.md) | DELETE | Remover um token de dispositivo do sistema |
| [Obter Estatísticas de Dispositivos](./DeviceStatsShow.md) | GET | Recuperar estatísticas de mensagens por tipo de dispositivo |

## Recursos

- **Mensagens em tempo real**: Enviar e receber mensagens instantaneamente
- **Notificações push**: Integração com Firebase Cloud Messaging
- **Reações a mensagens**: Reagir a mensagens com emojis
- **Confirmações de leitura**: Rastrear o status de entrega e leitura de mensagens
- **Rastreamento de dispositivos**: Monitorar mensagens por tipo de dispositivo (móvel, web, desktop)
- **Conversas polimórficas**: Associar conversas com diferentes entidades
- **Armazenamento em MongoDB**: Armazenamento de mensagens de alto desempenho

## Autenticação

Todos os endpoints requerem:
- Autenticação com Bearer token com habilidade `backoffice`
- Cabeçalho `X-PUBLIC-KEY` com a chave pública da plataforma
- Cabeçalho opcional `Accept-Language` para localização (pt-BR, en, es)

## URL Base

```
https://api.example.com/api/v1/pigeons/chat
```

## Documentação Relacionada

- [Visão Geral do Domínio Pigeons](../README.md)
- [Eventos de WebSocket/Broadcasting](../../Shared/README.md)
- [Guia de Configuração do Firebase](../../../../FIREBASE_SETUP.md)
