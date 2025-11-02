# Endpoints de la API de Chat Pigeons

Esta sección documenta todos los endpoints disponibles para el sistema de chat Pigeons, que proporciona mensajería en tiempo real, notificaciones push y gestión de dispositivos.

## Gestión de Conversaciones

| Endpoint | Método | Descripción |
|----------|--------|-------------|
| [Listar Conversaciones](./ConversationsIndex.md) | GET | Recuperar todas las conversaciones del usuario autenticado |
| [Crear Conversación](./ConversationsStore.md) | POST | Crear una nueva conversación con otro usuario |
| [Obtener Mensajes de Conversación](./ConversationMessagesIndex.md) | GET | Recuperar todos los mensajes de una conversación específica |

## Operaciones de Mensajes

| Endpoint | Método | Descripción |
|----------|--------|-------------|
| [Enviar Mensaje](./MessagesStore.md) | POST | Enviar un nuevo mensaje en una conversación |
| [Marcar Mensaje como Leído](./MessagesRead.md) | PATCH | Marcar un mensaje como leído por el usuario autenticado |
| [Agregar Reacción](./MessagesReactionsStore.md) | POST | Agregar una reacción emoji a un mensaje |

## Gestión de Dispositivos

| Endpoint | Método | Descripción |
|----------|--------|-------------|
| [Registrar Token de Dispositivo](./DevicesStore.md) | POST | Registrar un token de dispositivo FCM para notificaciones push |
| [Eliminar Token de Dispositivo](./DevicesDestroy.md) | DELETE | Eliminar un token de dispositivo del sistema |
| [Obtener Estadísticas de Dispositivos](./DeviceStatsShow.md) | GET | Recuperar estadísticas de mensajes por tipo de dispositivo |

## Características

- **Mensajería en tiempo real**: Enviar y recibir mensajes instantáneamente
- **Notificaciones push**: Integración con Firebase Cloud Messaging
- **Reacciones a mensajes**: Reaccionar a mensajes con emojis
- **Confirmaciones de lectura**: Rastrear el estado de entrega y lectura de mensajes
- **Seguimiento de dispositivos**: Monitorear mensajes por tipo de dispositivo (móvil, web, escritorio)
- **Conversaciones polimórficas**: Asociar conversaciones con diferentes entidades
- **Almacenamiento en MongoDB**: Almacenamiento de mensajes de alto rendimiento

## Autenticación

Todos los endpoints requieren:
- Autenticación con Bearer token con habilidad `backoffice`
- Encabezado `X-PUBLIC-KEY` con la clave pública de la plataforma
- Encabezado opcional `Accept-Language` para localización (pt-BR, en, es)

## URL Base

```
https://api.example.com/api/v1/pigeons/chat
```

## Documentación Relacionada

- [Resumen del Dominio Pigeons](../README.md)
- [Eventos de WebSocket/Broadcasting](../../Shared/README.md)
- [Guía de Configuración de Firebase](../../../../FIREBASE_SETUP.md)
