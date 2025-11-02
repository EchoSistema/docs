# Pigeons Chat API Endpoints

This section documents all available endpoints for the Pigeons chat system, which provides real-time messaging, push notifications, and device management.

## Conversation Management

| Endpoint | Method | Description |
|----------|--------|-------------|
| [List Conversations](./ConversationsIndex.md) | GET | Retrieve all conversations for the authenticated user |
| [Create Conversation](./ConversationsStore.md) | POST | Create a new conversation with another user |
| [Get Conversation Messages](./ConversationMessagesIndex.md) | GET | Retrieve all messages from a specific conversation |

## Message Operations

| Endpoint | Method | Description |
|----------|--------|-------------|
| [Send Message](./MessagesStore.md) | POST | Send a new message in a conversation |
| [Mark Message as Read](./MessagesRead.md) | PATCH | Mark a message as read by the authenticated user |
| [Add Reaction](./MessagesReactionsStore.md) | POST | Add an emoji reaction to a message |

## Device Management

| Endpoint | Method | Description |
|----------|--------|-------------|
| [Register Device Token](./DevicesStore.md) | POST | Register a FCM device token for push notifications |
| [Delete Device Token](./DevicesDestroy.md) | DELETE | Remove a device token from the system |
| [Get Device Statistics](./DeviceStatsShow.md) | GET | Retrieve message statistics by device type |

## Features

- **Real-time messaging**: Send and receive messages instantly
- **Push notifications**: Firebase Cloud Messaging integration
- **Message reactions**: React to messages with emojis
- **Read receipts**: Track message delivery and read status
- **Device tracking**: Monitor messages by device type (mobile, web, desktop)
- **Polymorphic conversations**: Associate conversations with different entities
- **MongoDB storage**: High-performance message storage

## Authentication

All endpoints require:
- Bearer token authentication with `backoffice` ability
- `X-PUBLIC-KEY` header with platform public key
- Optional `Accept-Language` header for localization (pt-BR, en, es)

## Base URL

```
https://api.example.com/api/v1/pigeons/chat
```

## Related Documentation

- [Pigeons Domain Overview](../README.md)
- [WebSocket/Broadcasting Events](../../Shared/README.md)
- [Firebase Setup Guide](../../../../FIREBASE_SETUP.md)
