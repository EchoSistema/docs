# Company

Serviços para acesso a dados de empresas dentro da plataforma EchoSistema.

## Visão Geral

O domínio Company fornece endpoints de API abrangentes para gerenciar informações de empresas, avaliações, oportunidades de emprego e cobertura geográfica. Este domínio permite que as plataformas exibam, filtrem e gerenciem dados relacionados a empresas, mantendo suporte multilíngue e configurações específicas da plataforma.

## Recursos Principais

- **Gestão de Empresas**: Recuperar e gerenciar perfis de empresas com informações completas
- **Gestão de Endereços**: Criar e gerenciar endereços de empresas com suporte a geolocalização
- **Grupos de Categorias**: Organizar empresas por categorias de negócio e áreas de domínio
- **Avaliações de Empresas**: Permitir que usuários avaliem e comentem sobre empresas
- **Gestão de Cobertura**: Filtrar e exibir empresas com base na cobertura geográfica
- **Oportunidades de Emprego**: Gerenciar e exibir vagas de emprego com requisitos detalhados

## Autenticação

Cada plataforma deve incluir o cabeçalho `X-PUBLIC-KEY` para autenticar e receber respostas. O frontend deve enviar o idioma atual da aplicação por meio do cabeçalho `Accept-Language` (ex.: `pt-BR`, `en`, `es`).

Endpoints protegidos requerem um token Bearer com habilidades apropriadas (ex.: `backoffice`, `store.company.address`).

## Cabeçalhos Comuns

- `X-PUBLIC-KEY`: Chave pública da plataforma (obrigatória para todos os endpoints)
- `Accept-Language`: Locale IETF para conteúdo traduzível (opcional, ex.: `pt-BR`, `en`, `es`)
- `Authorization`: Token Bearer para endpoints autenticados (obrigatório para rotas protegidas)

## Endpoints

Para documentação detalhada dos endpoints, consulte:

- [Diretório de Endpoints](./Endpoints/README.md)

## Recursos

O domínio Company inclui os seguintes recursos principais:

- **Company**: Perfil da empresa com integração de plataforma
- **CompanyAddress**: Endereços físicos e de faturamento
- **CompanyReview**: Avaliações e classificações de usuários
- **CategoryGroup**: Classificações de categorias de negócio
- **JobOpportunity**: Vagas e oportunidades de emprego

## Domínios Relacionados

- **Shared**: Fornece recursos comuns como endereços, categorias e plataformas
- **Bio**: Áreas de ocupação e informações profissionais

## Notas

- Todos os campos traduzíveis suportam múltiplos idiomas com fallback para o idioma padrão da plataforma
- Dados geográficos incluem país, estado e cidade com coordenadas de geolocalização
- Avaliações de empresas são calculadas automaticamente a partir das avaliações dos usuários
- Oportunidades de emprego suportam modos de trabalho remoto, híbrido e presencial
