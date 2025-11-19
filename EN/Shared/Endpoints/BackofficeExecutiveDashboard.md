# Shared – Backoffice Executive Dashboard

## Endpoint

```
GET /api/v1/backoffice/executive-dashboard
```

## Authentication

Requires valid Bearer token.

## Headers

| Header             | Type     | Required | Description |
| ------------------ | -------- | -------- | ----------- |
| Authorization      | string   | Yes      | `Bearer {token}` credential. |
| X-PUBLIC-KEY       | string   | Yes      | Platform public key. |
| Accept-Language    | string   | No       | IETF locale (e.g., `pt-BR`, `en`, `es`). |

## Permissions

- Gate `viewExecutiveDashboard`: Requires `backoffice.all` permission or be a backoffice user
- Backoffice users have automatic access
- Platform users must have `backoffice.all` permission in their roles

## Description

Returns the latest consolidated Executive Dashboard snapshot with system health overview, business metrics, and quick actions.

**Important:** Data is automatically generated every 10 minutes by the `ExecutiveDashboardDataGenerateJob` job. The endpoint **does not generate data on demand** - it only returns data from Redis cache or the most recent MongoDB persistence.

## Parameters

This endpoint has no parameters.

## Examples

### Request example (curl)

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: en" \
  "https://sandbox.your-domain.com/api/v1/backoffice/executive-dashboard"
```

### Response example

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

## HTTP Status

- 200: Success - Dashboard retrieved successfully
- 401: Unauthenticated - Invalid or missing token
- 403: Forbidden - `backoffice.all` permission required
- 500: Internal error

## Errors

### 401 Unauthenticated
```json
{
  "message": "Unauthenticated."
}
```

### 403 Forbidden
```json
{
  "message": "This action is unauthorized."
}
```

## Notes

### Data Updates
- Data is generated every **10 minutes** via scheduled job (`ExecutiveDashboardDataGenerateJob`)
- Cache maintained for 10 minutes using Redis
- MongoDB persistence for fallback if cache expires
- `executive-dashboard-updated` event is broadcast on `backoffice` channel after each update

### Permissions
- Requires `backoffice.all` permission or be a backoffice type user
- Validated via Policy `BackofficePolicy::viewExecutiveDashboard()`
- Backoffice users (identified via `isBackoffice()` method) have automatic access
- Platform users need `backoffice.all` in their role permissions

### Data Sources
- **Users by Area**: Aggregation of `users` → `platforms` → `platform_domain_areas`
- **Critical Alerts**: Unread messages in `platform_contact_messages`
- **System Notifications**: Analysis of last 500 lines from `storage/logs/laravel.log`
- **Revenue**: Sum of `payment_orders` with non-null `paid_at`
- **Resources**: Server metrics via `sys_getloadavg()`, `memory_get_usage()`, `disk_free_space()`

### WebSocket/Broadcasting
- Frontend can subscribe to `backoffice` channel for real-time updates
- Event: `executive-dashboard-updated`
- Event broadcast payload:
  ```json
  {
    "uuid": "platform-uuid",
    "generated_at": "2025-11-19T14:30:00.000000Z"
  }
  ```
- The event notifies when new data is available, allowing frontend to reload the dashboard
- Useful for dashboards that stay open for long periods

## Related

- [Backoffice - System Logs](./BackofficeLogs.md)
- [Backoffice - Platform Index](./BackofficePlatformIndex.md)
- [Backoffice - User Index](./BackofficePlatformUserIndex.md)

## Changelog

- 2025-11-19: Endpoint created with system, business metrics and quick actions
- 2025-11-19: Added system log analysis for notifications
- 2025-11-19: Implemented 10-minute scheduling and event broadcasting
