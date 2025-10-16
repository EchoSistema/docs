# Endpoints de PaymentHub

Este directorio contiene documentación detallada para todos los endpoints del dominio PaymentHub.

## Lista de Endpoints

### Órdenes de Pago

| Endpoint | Método | Descripción |
| -------- | ------ | ----------- |
| [PaymentOrderIndex](./PaymentOrderIndex.md) | GET | Listar todas las órdenes de pago de la plataforma autenticada |
| [PaymentOrderShow](./PaymentOrderShow.md) | GET | Recuperar información detallada sobre una orden de pago específica |

## Características Comunes

### Autenticación

Todos los endpoints requieren:
- `Authorization: Bearer {token}`
- `X-PUBLIC-KEY: {clave_plataforma}`

### Paginación

Los endpoints de listado soportan paginación:
- `per_page`: Número de resultados por página (predeterminado: 10)

### Formato de Respuesta

Todas las respuestas siguen el formato JSON estándar con envoltura `data` y metadatos de paginación cuando aplica.

## Documentación Relacionada

- [Resumen del Dominio PaymentHub](../README.md)
- [Dominio Ecommerce](../../Ecommerce/README.md)
