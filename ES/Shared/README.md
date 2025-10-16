# Documentación API del Dominio Shared

El dominio **Shared** proporciona endpoints API comunes utilizados en toda la plataforma EchoSistema. Estos endpoints manejan funcionalidades centrales como gestión de usuarios, ayudantes de autenticación, recursos de plataforma, mensajería de chat, formularios de contacto y gestión de contenido.

## Descripción General

Recursos comunes entre plataformas. Cada plataforma debe incluir el encabezado \`X-PUBLIC-KEY\` para autenticar y recibir respuestas. El frontend debe enviar el idioma actual de la aplicación mediante el encabezado \`Accept-Language\` (ej., \`en\`, \`pt-BR\`).

Este dominio contiene endpoints para:

- **Sistema de Chat**: Mensajería en tiempo real, conversaciones, reacciones y estadísticas de dispositivos
- **Gestión de Contactos**: Formularios de contacto de plataforma y manejo de mensajes
- **Pedidos**: Listado y detalles de pedidos
- **Imágenes**: Carga y gestión de imágenes
- **Perfil de Usuario**: Recuperación de perfil de usuario y configuración regional (idioma/moneda)
- **Direcciones**: Creación y gestión de direcciones
- **Categorías**: Gestión de categorías y grupos de categorías
- **Listas de Recursos**: Monedas, idiomas y roles disponibles
- **Boletín**: Gestión de suscripciones al boletín
- **Contraseña**: Funcionalidad de actualización de contraseña
- **Verificación de Salud**: Monitoreo del estado de salud de la API
- **Usuarios de Plataforma**: Gestión de usuarios de plataforma y asignación de roles
- **Backoffice**: Endpoints administrativos para áreas de dominio, plataformas y usuarios
- **Gestión de Plataforma**: Configuración de email, imágenes, textos, títulos y contenido popular

## Autenticación

La mayoría de los endpoints requieren autenticación usando tokens Bearer:

\`\`\`
Authorization: Bearer {token}
\`\`\`

Algunos endpoints también requieren habilidades específicas (ej., \`backoffice\`) para control de acceso.

## Encabezados Comunes

| Encabezado      | Requerido | Descripción |
| --------------- | --------- | ----------- |
| Authorization   | Varía     | Token Bearer para solicitudes autenticadas |
| X-PUBLIC-KEY    | Sí        | Clave pública de plataforma para todas las solicitudes |
| Accept-Language | No        | Locale IETF (ej., \`pt-BR\`, \`en\`, \`es\`) para contenido localizado |

## Documentación de Endpoints

Para documentación detallada de endpoints, consulte el directorio [Endpoints](./Endpoints/README.md).

## Enlaces Rápidos

- [Endpoints de Chat](./Endpoints/README.md#endpoints-de-chat)
- [Endpoints de Contacto](./Endpoints/README.md#endpoints-de-contacto)
- [Gestión de Usuarios](./Endpoints/README.md#endpoints-de-usuario)
- [Gestión de Plataforma](./Endpoints/README.md#endpoints-de-gestión-de-plataforma)

## Soporte

Para soporte de API o actualizaciones de documentación:
- Repositorio de Documentación: https://github.com/EchoSistema/docs
- Email de Soporte: api-support@echosistema.com
