# Endpoints API do Domínio Shared

Lista completa de todos os endpoints disponíveis no domínio Shared, organizados por categoria funcional.

## Overview

Total: 54 endpoints across 12 categories.

## Endpoints de Chat

Mensagens em tempo real e gestão de conversas

- [Listar Conversas de Chat](./ChatListConversations.md)
- [Criar Conversa de Chat](./ChatCreateConversation.md)
- [Obter Mensagens da Conversa](./ChatGetMessages.md)
- [Enviar Mensagem de Chat](./ChatSendMessage.md)
- [Marcar Mensagem como Lida](./ChatMarkAsRead.md)
- [Adicionar Reação à Mensagem](./ChatAddReaction.md)
- [Obter Estatísticas de Dispositivos](./ChatGetDeviceStats.md)

## Endpoints de Contato

Formulários de contato e gestão de mensagens

- [Criar Mensagem de Contato](./PlatformContactMessageStore.md)
- [Enviar Imagem de Mensagem de Contato](./PlatformContactMessageStoreImage.md)
- [Listar Mensagens de Contato](./PlatformContactMessageIndex.md)
- [Exibir Mensagem de Contato](./PlatformContactMessageShow.md)
- [Alternar Status de Leitura da Mensagem](./PlatformContactMessageToggleRead.md)

## Endpoints de Pedidos

Gestão de pedidos e detalhes

- [Listar Pedidos](./OrderIndex.md)
- [Exibir Detalhes do Pedido](./OrderShow.md)

## Endpoints de Imagens

Upload e gestão de imagens

- [Enviar Imagem](./ImageStore.md)
- [Excluir Imagem](./ImageDestroy.md)

## Endpoints de Usuário e Perfil

Perfil de usuário e configurações regionais

- [Obter Perfil do Usuário](./UserProfile.md)
- [Atualizar Meu Perfil](./UserProfileUpdate.md)
- [Atualizar Usuário (Admin)](./AdminUserUpdate.md)
- [Atualizar Idioma do Usuário](./UserLanguageUpdate.md)
- [Atualizar Moeda do Usuário](./UserCurrencyUpdate.md)

## Endpoints de Endereço

Criação e gestão de endereços

- [Criar Endereço](./AddressStore.md)

## Endpoints de Categorias

Gestão de categorias e grupos de categorias

- [Listar Categorias](./CategoryIndex.md)
- [Listar Grupos de Categorias](./CategoryGroupIndex.md)
- [Criar Grupo de Categorias](./CategoryGroupStore.md)

## Endpoints de Listas de Recursos

Moedas, idiomas e papéis disponíveis

- [Listar Moedas Disponíveis](./CurrencyIndex.md)
- [Listar Idiomas Disponíveis](./LanguageIndex.md)
- [Listar Papéis Disponíveis](./RoleIndex.md)

## Outros Endpoints

Newsletter, senha e verificação de saúde

- [Inscrever-se na Newsletter](./NewsletterStore.md)
- [Atualizar Senha](./PasswordUpdate.md)
- [Verificação de Saúde](./HealthCheck.md)

## Endpoints de Usuários da Plataforma

Gestão de usuários da plataforma e atribuição de papéis

- [Listar Usuários da Plataforma](./PlatformUserIndex.md)
- [Exibir Usuário da Plataforma](./PlatformUserShow.md)
- [Contar Usuários da Plataforma por Papel](./PlatformUserCounter.md)
- [Criar Usuário da Plataforma](./PlatformUserStore.md)
- [Atualizar Usuário da Plataforma](./PlatformUserUpdate.md)
- [Atualizar Papel do Usuário da Plataforma](./PlatformUserUpdateRole.md)
- [Remover Papel do Usuário da Plataforma](./PlatformUserRemoveRole.md)

## Endpoints de Backoffice

Endpoints administrativos (requer habilidade backoffice)

- [Listar Áreas de Domínio](./BackofficeDomainAreaIndex.md)
- [Listar Plataformas](./BackofficePlatformIndex.md)
- [Exibir Detalhes da Plataforma](./BackofficePlatformShow.md)
- [Listar Todos os Usuários da Plataforma](./BackofficePlatformUserIndex.md)
- [Registrar Nova Plataforma](./PlatformAuthenticationRegister.md)

## Endpoints de Gestão de Plataforma

Gestão de email, imagens, textos, títulos e conteúdo

- [Criar Configuração de Email](./PlatformEmailConfigStore.md)
- [Exibir Configuração de Email](./PlatformEmailConfigShow.md)
- [Atualizar Configuração de Email](./PlatformEmailConfigUpdate.md)
- [Listar Emails da Plataforma](./PlatformEmailIndex.md)
- [Exibir Email da Plataforma](./PlatformEmailShow.md)
- [Listar Imagens da Plataforma](./PlatformImageIndex.md)
- [Listar Textos da Plataforma](./PlatformTextIndex.md)
- [Listar Títulos da Plataforma](./PlatformTitleIndex.md)
- [Criar Título da Plataforma](./PlatformTitleStore.md)
- [Exibir Título da Plataforma](./PlatformTitleShow.md)
- [Atualizar Título da Plataforma](./PlatformTitleUpdate.md)
- [Excluir Título da Plataforma](./PlatformTitleDestroy.md)
- [Listar Conteúdo Mais Popular](./MostPopularContentIndex.md)
