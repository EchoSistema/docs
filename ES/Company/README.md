# Company

Servicios para acceder a datos de empresas dentro de la plataforma EchoSistema.

## Descripción General

El dominio Company proporciona endpoints de API completos para gestionar información de empresas, reseñas, oportunidades de empleo y cobertura geográfica. Este dominio permite a las plataformas mostrar, filtrar y gestionar datos relacionados con empresas manteniendo soporte multiidioma y configuraciones específicas de la plataforma.

## Características Principales

- **Gestión de Empresas**: Recuperar y gestionar perfiles de empresas con información completa
- **Gestión de Direcciones**: Crear y gestionar direcciones de empresas con soporte de geolocalización
- **Grupos de Categorías**: Organizar empresas por categorías de negocio y áreas de dominio
- **Reseñas de Empresas**: Permitir a los usuarios calificar y reseñar empresas
- **Gestión de Cobertura**: Filtrar y mostrar empresas según cobertura geográfica
- **Oportunidades de Empleo**: Gestionar y mostrar publicaciones de empleo con requisitos detallados

## Autenticación

Cada plataforma debe incluir el encabezado `X-PUBLIC-KEY` para autenticarse y recibir respuestas. El frontend debe enviar el idioma actual de la aplicación mediante el encabezado `Accept-Language` (ej.: `pt-BR`, `en`, `es`).

Los endpoints protegidos requieren un token Bearer con habilidades apropiadas (ej.: `backoffice`, `store.company.address`).

## Encabezados Comunes

- `X-PUBLIC-KEY`: Clave pública de la plataforma (requerida para todos los endpoints)
- `Accept-Language`: Locale IETF para contenido traducible (opcional, ej.: `pt-BR`, `en`, `es`)
- `Authorization`: Token Bearer para endpoints autenticados (requerido para rutas protegidas)

## Endpoints

Para documentación detallada de endpoints, consulte:

- [Directorio de Endpoints](./Endpoints/README.md)

## Recursos

El dominio Company incluye los siguientes recursos principales:

- **Company**: Perfil de empresa con integración de plataforma
- **CompanyAddress**: Direcciones físicas y de facturación
- **CompanyReview**: Reseñas y calificaciones de usuarios
- **CategoryGroup**: Clasificaciones de categorías de negocio
- **JobOpportunity**: Publicaciones y ofertas de empleo

## Dominios Relacionados

- **Shared**: Proporciona recursos comunes como direcciones, categorías y plataformas
- **Bio**: Áreas de ocupación e información profesional

## Notas

- Todos los campos traducibles soportan múltiples idiomas con respaldo al idioma predeterminado de la plataforma
- Los datos geográficos incluyen país, estado y ciudad con coordenadas de geolocalización
- Las calificaciones de empresas se calculan automáticamente a partir de reseñas de usuarios
- Las oportunidades de empleo soportan modos de trabajo remoto, híbrido y presencial
