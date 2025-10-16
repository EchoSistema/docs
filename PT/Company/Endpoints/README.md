# Endpoints de Company

Este diretório contém documentação para todos os endpoints da API do domínio Company.

## Endpoints Disponíveis

### Gestão de Empresas

- [**Exibir Empresa**](./CompanyShow.md) - `GET /api/v1/company`
  - Recuperar detalhes da empresa autenticada

### Endereços de Empresa

- [**Cadastrar Endereço de Empresa**](./CompanyAddressStore.md) - `POST /api/v1/company/{addressable}/addresses`
  - Criar um novo endereço para uma empresa

### Grupos de Categorias

- [**Listar Grupos de Categorias**](./CompanyCategoryGroupIndex.md) - `GET /api/v1/company/category-groups`
  - Listar todos os grupos de categorias disponíveis para a plataforma

### Avaliações de Empresas

- [**Listar Avaliações de Empresas**](./CompanyReviewIndex.md) - `GET /api/v1/company/reviews`
  - Listar todas as avaliações de empresas com opções de filtragem

- [**Criar Avaliação de Empresa**](./CompanyReviewStore.md) - `POST /api/v1/company/reviews`
  - Criar uma nova avaliação para uma empresa

### Cobertura de Empresas

- [**Índice de Empresas Através de Cobertura**](./CompanyThroughCoverageIndex.md) - `GET /api/v1/company/through-coverage`
  - Listar empresas disponíveis através da cobertura da plataforma

- [**Geolocalizações de Empresas**](./CompanyThroughCoverageGeolocations.md) - `GET /api/v1/company/through-coverage/geolocations`
  - Obter dados de geolocalização de empresas

- [**Contador de Empresas**](./CompanyThroughCoverageCounter.md) - `GET /api/v1/company/through-coverage/counter`
  - Obter contagem de empresas através da cobertura

### Oportunidades de Emprego

- [**Listar Oportunidades de Emprego**](./MeJobOpportunityIndex.md) - `GET /api/v1/company/opportunities`
  - Listar todas as oportunidades de emprego da empresa

- [**Exibir Oportunidade de Emprego**](./MeJobOpportunityShow.md) - `GET /api/v1/company/opportunities/{jobOpportunity}`
  - Obter informações detalhadas sobre uma oportunidade de emprego específica

## Autenticação

A maioria dos endpoints requer autenticação via token Bearer. Endpoints públicos requerem apenas o cabeçalho `X-PUBLIC-KEY`.

## Cabeçalhos Comuns

Todos os endpoints aceitam estes cabeçalhos comuns:

- `X-PUBLIC-KEY` - Chave pública da plataforma (obrigatória para todos os endpoints)
- `Accept-Language` - Preferência de idioma para conteúdo traduzível (opcional)
- `Authorization` - Token Bearer para endpoints autenticados (obrigatório para endpoints protegidos)

## Formato de Resposta

Todos os endpoints retornam respostas JSON seguindo as convenções do Laravel Resource com um wrapper `data`.

Endpoints paginados incluem objetos `links` e `meta` para navegação e informações de paginação.

## Documentação Relacionada

- [Visão Geral do Domínio Company](../README.md)
- [Guia de Estilo da API](../../../STYLEGUIDE.md)
