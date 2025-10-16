# Documentação API do Domínio Learn

O domínio **Learn** fornece endpoints API para gerenciamento educacional e de eventos dentro da plataforma EchoSistema. Esses endpoints lidam com listagem de eventos, gerenciamento de conteúdo, categorias e recursos de visualização de eventos.

## Visão Geral

Gerenciamento de conteúdo educacional e eventos. Cada plataforma deve incluir o cabeçalho `X-PUBLIC-KEY` para autenticar e receber respostas. O frontend deve enviar o idioma atual da aplicação através do cabeçalho `Accept-Language` (ex.: `en`, `pt-BR`).

Este domínio contém endpoints para:

- **Eventos**: Listagem de eventos, detalhes, contadores e recuperação de conteúdo de texto
- **Conteúdos de Eventos**: Gerenciamento de conteúdo e sub-conteúdo para eventos
- **Tipos de Conteúdo de Eventos**: Tipos de conteúdo disponíveis para eventos
- **Visualizações de Eventos**: Views materializadas para dados de eventos
- **Categorias Learn**: Gerenciamento de categorias para o domínio Learn
- **Grupos de Categorias Learn**: Listagem e detalhes de grupos de categorias

## Autenticação

A maioria dos endpoints requer autenticação usando tokens Bearer:

```
Authorization: Bearer {token}
```

Alguns endpoints podem funcionar sem autenticação, mas requerem a identificação da plataforma através de `X-PUBLIC-KEY`.

## Cabeçalhos Comuns

| Cabeçalho       | Obrigatório | Descrição |
| --------------- | ----------- | --------- |
| Authorization   | Varia       | Token Bearer para requisições autenticadas |
| X-PUBLIC-KEY    | Sim         | Chave pública da plataforma para todas as requisições |
| Accept-Language | Não         | Locale IETF (ex.: `pt-BR`, `en`, `es`) para conteúdo localizado |

## Documentação dos Endpoints

Para documentação detalhada dos endpoints, consulte o diretório [Endpoints](./Endpoints/README.md).

## Links Rápidos

- [Endpoints de Eventos](./Endpoints/README.md#endpoints-de-eventos)
- [Endpoints de Conteúdo de Eventos](./Endpoints/README.md#endpoints-de-conteúdo-de-eventos)
- [Endpoints de Categorias](./Endpoints/README.md#endpoints-de-categorias)

## Suporte

Para suporte de API ou atualizações de documentação:
- Repositório de Documentação: https://github.com/EchoSistema/docs
- Email de Suporte: api-support@echosistema.com
