# Dominio PaymentHub

El dominio PaymentHub gestiona la administración y recuperación de órdenes de pago para la plataforma.

## Resumen

Este dominio proporciona endpoints para:
- Listar órdenes de pago asociadas con una plataforma
- Recuperar información detallada sobre órdenes de pago específicas
- Gestionar transacciones de pago y cuotas

## Autenticación

Todos los endpoints requieren autenticación mediante token Bearer con el encabezado `X-PUBLIC-KEY`.

## Ruta Base

```
/api/v1/payment-hub
```

## Endpoints Disponibles

- [Índice de Órdenes de Pago](./Endpoints/PaymentOrderIndex.md) - Listar todas las órdenes de pago de la plataforma
- [Mostrar Orden de Pago](./Endpoints/PaymentOrderShow.md) - Obtener detalles de una orden de pago específica

## Recursos

### PaymentOrder

Una orden de pago representa una transacción o intención de pago creada por un usuario de la plataforma.

**Atributos clave:**
- `uuid`: Identificador único
- `status`: Estado actual del pago
- `amount`: Monto del pago
- `currency`: Moneda del pago
- `installments`: Detalles de cuotas de pago (cuando aplica)

## Dominios Relacionados

- **Shared**: Recursos comunes y autenticación
- **Ecommerce**: Compras de productos que generan órdenes de pago

## Notas

- Las órdenes de pago están delimitadas por plataformas
- Los datos de cuotas solo se cargan al ver una orden de pago específica
- La paginación está disponible con tamaño de página configurable (predeterminado: 10)

## Soporte

Para preguntas o problemas relacionados con los endpoints de PaymentHub, consulte la documentación individual de cada endpoint o contacte al equipo de desarrollo.
