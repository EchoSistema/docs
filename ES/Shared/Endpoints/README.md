# Endpoints API del Dominio Shared

Lista completa de todos los endpoints disponibles en el dominio Shared, organizados por categoría funcional.

## Overview

Total: 54 endpoints across 12 categories.

## Endpoints de Chat

Mensajería en tiempo real y gestión de conversaciones

- [Listar Conversaciones de Chat](./ChatListConversations.md)
- [Crear Conversación de Chat](./ChatCreateConversation.md)
- [Obtener Mensajes de Conversación](./ChatGetMessages.md)
- [Enviar Mensaje de Chat](./ChatSendMessage.md)
- [Marcar Mensaje como Leído](./ChatMarkAsRead.md)
- [Agregar Reacción al Mensaje](./ChatAddReaction.md)
- [Obtener Estadísticas de Dispositivos](./ChatGetDeviceStats.md)

## Endpoints de Contacto

Formularios de contacto y gestión de mensajes

- [Crear Mensaje de Contacto](./PlatformContactMessageStore.md)
- [Subir Imagen de Mensaje de Contacto](./PlatformContactMessageStoreImage.md)
- [Listar Mensajes de Contacto](./PlatformContactMessageIndex.md)
- [Mostrar Mensaje de Contacto](./PlatformContactMessageShow.md)
- [Alternar Estado de Lectura de Mensaje](./PlatformContactMessageToggleRead.md)

## Endpoints de Pedidos

Gestión de pedidos y detalles

- [Listar Pedidos](./OrderIndex.md)
- [Mostrar Detalles del Pedido](./OrderShow.md)

## Endpoints de Imágenes

Carga y gestión de imágenes

- [Subir Imagen](./ImageStore.md)
- [Eliminar Imagen](./ImageDestroy.md)

## Endpoints de Usuario y Perfil

Perfil de usuario y configuración regional

- [Obtener Perfil de Usuario](./UserProfile.md)
- [Actualizar Idioma del Usuario](./UserLanguageUpdate.md)
- [Actualizar Moneda del Usuario](./UserCurrencyUpdate.md)

## Endpoints de Dirección

Creación y gestión de direcciones

- [Crear Dirección](./AddressStore.md)

## Endpoints de Categorías

Gestión de categorías y grupos de categorías

- [Listar Categorías](./CategoryIndex.md)
- [Listar Grupos de Categorías](./CategoryGroupIndex.md)
- [Crear Grupo de Categorías](./CategoryGroupStore.md)

## Endpoints de Listas de Recursos

Monedas, idiomas y roles disponibles

- [Listar Monedas Disponibles](./CurrencyIndex.md)
- [Listar Idiomas Disponibles](./LanguageIndex.md)
- [Listar Roles Disponibles](./RoleIndex.md)

## Otros Endpoints

Boletín, contraseña y verificación de salud

- [Suscribirse al Boletín](./NewsletterStore.md)
- [Actualizar Contraseña](./PasswordUpdate.md)
- [Verificación de Salud](./HealthCheck.md)

## Endpoints de Usuarios de Plataforma

Gestión de usuarios de plataforma y asignación de roles

- [Listar Usuarios de Plataforma](./PlatformUserIndex.md)
- [Mostrar Usuario de Plataforma](./PlatformUserShow.md)
- [Contar Usuarios de Plataforma por Rol](./PlatformUserCounter.md)
- [Crear Usuario de Plataforma](./PlatformUserStore.md)
- [Actualizar Usuario de Plataforma](./PlatformUserUpdate.md)
- [Actualizar Rol de Usuario de Plataforma](./PlatformUserUpdateRole.md)
- [Eliminar Rol de Usuario de Plataforma](./PlatformUserRemoveRole.md)

## Endpoints de Backoffice

Endpoints administrativos (requiere habilidad backoffice)

- [Listar Áreas de Dominio](./BackofficeDomainAreaIndex.md)
- [Listar Plataformas](./BackofficePlatformIndex.md)
- [Mostrar Detalles de Plataforma](./BackofficePlatformShow.md)
- [Listar Todos los Usuarios de Plataforma](./BackofficePlatformUserIndex.md)
- [Registrar Nueva Plataforma](./PlatformAuthenticationRegister.md)

## Endpoints de Gestión de Plataforma

Gestión de email, imágenes, textos, títulos y contenido

- [Crear Configuración de Email](./PlatformEmailConfigStore.md)
- [Mostrar Configuración de Email](./PlatformEmailConfigShow.md)
- [Actualizar Configuración de Email](./PlatformEmailConfigUpdate.md)
- [Listar Emails de Plataforma](./PlatformEmailIndex.md)
- [Mostrar Email de Plataforma](./PlatformEmailShow.md)
- [Listar Imágenes de Plataforma](./PlatformImageIndex.md)
- [Listar Textos de Plataforma](./PlatformTextIndex.md)
- [Listar Títulos de Plataforma](./PlatformTitleIndex.md)
- [Crear Título de Plataforma](./PlatformTitleStore.md)
- [Mostrar Título de Plataforma](./PlatformTitleShow.md)
- [Actualizar Título de Plataforma](./PlatformTitleUpdate.md)
- [Eliminar Título de Plataforma](./PlatformTitleDestroy.md)
- [Listar Contenido Más Popular](./MostPopularContentIndex.md)

