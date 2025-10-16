# Endpoints de Company

Este directorio contiene documentación para todos los endpoints de la API del dominio Company.

## Endpoints Disponibles

### Gestión de Empresas

- [**Mostrar Empresa**](./CompanyShow.md) - `GET /api/v1/company`
  - Recuperar detalles de la empresa autenticada

### Direcciones de Empresa

- [**Registrar Dirección de Empresa**](./CompanyAddressStore.md) - `POST /api/v1/company/{addressable}/addresses`
  - Crear una nueva dirección para una empresa

### Grupos de Categorías

- [**Listar Grupos de Categorías**](./CompanyCategoryGroupIndex.md) - `GET /api/v1/company/category-groups`
  - Listar todos los grupos de categorías disponibles para la plataforma

### Reseñas de Empresas

- [**Listar Reseñas de Empresas**](./CompanyReviewIndex.md) - `GET /api/v1/company/reviews`
  - Listar todas las reseñas de empresas con opciones de filtrado

- [**Crear Reseña de Empresa**](./CompanyReviewStore.md) - `POST /api/v1/company/reviews`
  - Crear una nueva reseña para una empresa

### Cobertura de Empresas

- [**Índice de Empresas a Través de Cobertura**](./CompanyThroughCoverageIndex.md) - `GET /api/v1/company/through-coverage`
  - Listar empresas disponibles a través de la cobertura de la plataforma

- [**Geolocalizaciones de Empresas**](./CompanyThroughCoverageGeolocations.md) - `GET /api/v1/company/through-coverage/geolocations`
  - Obtener datos de geolocalización de empresas

- [**Contador de Empresas**](./CompanyThroughCoverageCounter.md) - `GET /api/v1/company/through-coverage/counter`
  - Obtener recuento de empresas a través de la cobertura

### Oportunidades de Empleo

- [**Listar Oportunidades de Empleo**](./MeJobOpportunityIndex.md) - `GET /api/v1/company/opportunities`
  - Listar todas las oportunidades de empleo de la empresa

- [**Mostrar Oportunidad de Empleo**](./MeJobOpportunityShow.md) - `GET /api/v1/company/opportunities/{jobOpportunity}`
  - Obtener información detallada sobre una oportunidad de empleo específica

## Autenticación

La mayoría de los endpoints requieren autenticación mediante token Bearer. Los endpoints públicos solo requieren el encabezado `X-PUBLIC-KEY`.

## Encabezados Comunes

Todos los endpoints aceptan estos encabezados comunes:

- `X-PUBLIC-KEY` - Clave pública de la plataforma (requerida para todos los endpoints)
- `Accept-Language` - Preferencia de idioma para contenido traducible (opcional)
- `Authorization` - Token Bearer para endpoints autenticados (requerido para endpoints protegidos)

## Formato de Respuesta

Todos los endpoints devuelven respuestas JSON siguiendo las convenciones de Laravel Resource con un envoltorio `data`.

Los endpoints paginados incluyen objetos `links` y `meta` para navegación e información de paginación.

## Documentación Relacionada

- [Descripción General del Dominio Company](../README.md)
- [Guía de Estilo de API](../../../STYLEGUIDE.md)
