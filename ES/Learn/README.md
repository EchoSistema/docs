# Documentación API del Dominio Learn

El dominio **Learn** proporciona endpoints API para gestión educativa y de eventos dentro de la plataforma EchoSistema. Estos endpoints manejan listados de eventos, gestión de contenido, categorías y características de visualización de eventos.

## Descripción General

Gestión de contenido educativo y eventos. Cada plataforma debe incluir el encabezado `X-PUBLIC-KEY` para autenticar y recibir respuestas. El frontend debe enviar el idioma actual de la aplicación mediante el encabezado `Accept-Language` (ej., `en`, `pt-BR`).

Este dominio contiene endpoints para:

- **Eventos**: Listado de eventos, detalles, contadores y recuperación de contenido de texto
- **Contenidos de Eventos**: Gestión de contenido y sub-contenido para eventos
- **Tipos de Contenido de Eventos**: Tipos de contenido disponibles para eventos
- **Vistas de Eventos**: Vistas materializadas para datos de eventos
- **Categorías Learn**: Gestión de categorías para el dominio Learn
- **Grupos de Categorías Learn**: Listado y detalles de grupos de categorías

## Autenticación

La mayoría de los endpoints requieren autenticación usando tokens Bearer:

```
Authorization: Bearer {token}
```

Algunos endpoints pueden funcionar sin autenticación pero requieren la identificación de plataforma a través de `X-PUBLIC-KEY`.

## Encabezados Comunes

| Encabezado      | Requerido | Descripción |
| --------------- | --------- | ----------- |
| Authorization   | Varía     | Token Bearer para solicitudes autenticadas |
| X-PUBLIC-KEY    | Sí        | Clave pública de plataforma para todas las solicitudes |
| Accept-Language | No        | Locale IETF (ej., `pt-BR`, `en`, `es`) para contenido localizado |

## Documentación de Endpoints

Para documentación detallada de endpoints, consulte el directorio [Endpoints](./Endpoints/README.md).

## Enlaces Rápidos

- [Endpoints de Eventos](./Endpoints/README.md#endpoints-de-eventos)
- [Endpoints de Contenido de Eventos](./Endpoints/README.md#endpoints-de-contenido-de-eventos)
- [Endpoints de Categorías](./Endpoints/README.md#endpoints-de-categorías)

## Soporte

Para soporte de API o actualizaciones de documentación:
- Repositorio de Documentación: https://github.com/EchoSistema/docs
- Email de Soporte: api-support@echosistema.com
