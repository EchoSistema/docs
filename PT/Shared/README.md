# Documentação API do Domínio Shared

O domínio **Shared** fornece endpoints API comuns utilizados em toda a plataforma EchoSistema. Esses endpoints lidam com funcionalidades centrais como gestão de usuários, auxiliares de autenticação, recursos de plataforma, mensagens de chat, formulários de contato e gestão de conteúdo.

## Visão Geral

Recursos comuns entre plataformas. Cada plataforma deve incluir o cabeçalho \`X-PUBLIC-KEY\` para autenticar e receber respostas. O frontend deve enviar o idioma atual da aplicação através do cabeçalho \`Accept-Language\` (ex.: \`en\`, \`pt-BR\`).

Este domínio contém endpoints para:

- **Sistema de Chat**: Mensagens em tempo real, conversas, reações e estatísticas de dispositivos
- **Gestão de Contatos**: Formulários de contato da plataforma e tratamento de mensagens
- **Pedidos**: Listagem e detalhes de pedidos
- **Imagens**: Upload e gestão de imagens
- **Perfil de Usuário**: Recuperação de perfil de usuário e configurações regionais (idioma/moeda)
- **Endereços**: Criação e gestão de endereços
- **Categorias**: Gestão de categorias e grupos de categorias
- **Listas de Recursos**: Moedas, idiomas e papéis disponíveis
- **Newsletter**: Gestão de assinaturas de newsletter
- **Senha**: Funcionalidade de atualização de senha
- **Verificação de Saúde**: Monitoramento do status de saúde da API
- **Usuários da Plataforma**: Gestão de usuários da plataforma e atribuição de papéis
- **Backoffice**: Endpoints administrativos para áreas de domínio, plataformas e usuários
- **Gestão de Plataforma**: Configuração de email, imagens, textos, títulos e conteúdo popular

## Autenticação

A maioria dos endpoints requer autenticação usando tokens Bearer:

\`\`\`
Authorization: Bearer {token}
\`\`\`

Alguns endpoints também requerem habilidades específicas (ex.: \`backoffice\`) para controle de acesso.

## Cabeçalhos Comuns

| Cabeçalho       | Obrigatório | Descrição |
| --------------- | ----------- | --------- |
| Authorization   | Varia       | Token Bearer para requisições autenticadas |
| X-PUBLIC-KEY    | Sim         | Chave pública da plataforma para todas as requisições |
| Accept-Language | Não         | Locale IETF (ex.: \`pt-BR\`, \`en\`, \`es\`) para conteúdo localizado |

## Documentação dos Endpoints

Para documentação detalhada dos endpoints, consulte o diretório [Endpoints](./Endpoints/README.md).

## Links Rápidos

- [Endpoints de Chat](./Endpoints/README.md#endpoints-de-chat)
- [Endpoints de Contato](./Endpoints/README.md#endpoints-de-contato)
- [Gestão de Usuários](./Endpoints/README.md#endpoints-de-usuário)
- [Gestão de Plataforma](./Endpoints/README.md#endpoints-de-gestão-de-plataforma)

## Suporte

Para suporte de API ou atualizações de documentação:
- Repositório de Documentação: https://github.com/EchoSistema/docs
- Email de Suporte: api-support@echosistema.com
