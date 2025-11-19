# Shared – Panel Ejecutivo del Backoffice

## Endpoint

```
GET /api/v1/backoffice/executive-dashboard
```

## Autenticación

Requiere token Bearer válido.

## Encabezados

| Encabezado         | Tipo     | Obligatorio | Descripción |
| ------------------ | -------- | ----------- | ----------- |
| Authorization      | string   | Sí          | Credencial `Bearer {token}`. |
| X-PUBLIC-KEY       | string   | Sí          | Clave pública de la plataforma. |
| Accept-Language    | string   | No          | Locale IETF (ej.: `pt-BR`, `en`, `es`). |

## Permisos

- Gate `viewExecutiveDashboard`: Requiere permiso `backoffice.all` o ser usuario backoffice
- Los usuarios backoffice tienen acceso automático
- Los usuarios de plataforma deben tener el permiso `backoffice.all` en sus roles

## Descripción

Retorna el último snapshot consolidado del Panel Ejecutivo con visión de salud del sistema, métricas de negocio y acciones rápidas.

**Importante:** Los datos se generan automáticamente cada 10 minutos por el job `ExecutiveDashboardDataGenerateJob`. El endpoint **no genera datos bajo demanda** - solo retorna datos del cache Redis o de la persistencia MongoDB más reciente.

## Parámetros

Este endpoint no tiene parámetros.

## Ejemplos

### Ejemplo de solicitud (curl)

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <clave>" \
  -H "Accept-Language: es" \
  "https://sandbox.su-dominio.com/api/v1/backoffice/executive-dashboard"
```

### Ejemplo de respuesta

```json
{
  "data": {
    "generated_at": "2025-11-19T14:30:00.000000Z",
    "system_health": {
      "domain_areas": [
        {
          "domain_area": "ecommerce",
          "name": "E-commerce",
          "users": 1250,
          "status": "operational"
        }
      ],
      "critical_alerts": {
        "total_unread": 3,
        "latest": []
      },
      "uptime": {
        "last_24h": null,
        "last_7d": null,
        "last_30d": null
      },
      "resource_utilization": {
        "cpu_percentage": 45.23,
        "memory": {
          "used": 536870912,
          "limit": 2147483648,
          "percentage": 25.0
        }
      }
    },
    "business_metrics": {
      "active_users": 5420,
      "new_users_month": 320,
      "growth_rate_mom": 6.25,
      "revenue_total_cents": 1500000000,
      "revenue_month_cents": 85000000
    },
    "quick_actions": {
      "deploy_status": {
        "stage": "Production",
        "environment": "production"
      },
      "pending_approvals": 12,
      "system_notifications": {
        "error_count": 5,
        "warning_count": 12,
        "critical_count": 1
      }
    }
  }
}
```

## Estado HTTP

- 200: Éxito - Panel recuperado con éxito
- 401: No autenticado - Token inválido o ausente
- 403: Prohibido - Permiso `backoffice.all` requerido
- 500: Error interno

## Errores

### 401 No autenticado
```json
{
  "message": "No autenticado."
}
```

### 403 Prohibido
```json
{
  "message": "Esta acción no está autorizada."
}
```

## Notas

### Actualización de Datos
- Los datos se generan cada **10 minutos** via job programado (`ExecutiveDashboardDataGenerateJob`)
- Cache mantenido por 10 minutos usando Redis
- Persistencia MongoDB para respaldo si el cache expira
- El evento `executive-dashboard-updated` se transmite en el canal `backoffice` después de cada actualización

### Permisos
- Requiere permiso `backoffice.all` o ser usuario tipo backoffice
- Validado via Policy `BackofficePolicy::viewExecutiveDashboard()`
- Los usuarios backoffice (identificados via método `isBackoffice()`) tienen acceso automático
- Los usuarios de plataforma necesitan `backoffice.all` en los permisos de sus roles

### Fuentes de Datos
- **Usuarios por Área**: Agregación de `users` → `platforms` → `platform_domain_areas`
- **Alertas Críticas**: Mensajes no leídos en `platform_contact_messages`
- **Notificaciones del Sistema**: Análisis de las últimas 500 líneas de `storage/logs/laravel.log`
- **Ingresos**: Suma de `payment_orders` con `paid_at` no nulo
- **Recursos**: Métricas del servidor via `sys_getloadavg()`, `memory_get_usage()`, `disk_free_space()`

### WebSocket/Broadcasting
- El frontend puede suscribirse al canal `backoffice` para actualizaciones en tiempo real
- Evento: `executive-dashboard-updated`
- Payload del evento broadcast:
  ```json
  {
    "uuid": "platform-uuid",
    "generated_at": "2025-11-19T14:30:00.000000Z"
  }
  ```
- El evento notifica cuando nuevos datos están disponibles, permitiendo al frontend recargar el panel
- Útil para paneles que permanecen abiertos por largos períodos

## Relacionados

- [Backoffice - Logs del Sistema](./BackofficeLogs.md)
- [Backoffice - Índice de Plataformas](./BackofficePlatformIndex.md)
- [Backoffice - Índice de Usuarios](./BackofficePlatformUserIndex.md)

## Changelog

- 2025-11-19: Endpoint creado con métricas de sistema, negocio y acciones rápidas
- 2025-11-19: Agregado análisis de logs del sistema para notificaciones
- 2025-11-19: Implementado programación cada 10 minutos y broadcasting de eventos
