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
        "total": 2,
        "latest": [
          {
            "level": "ERROR",
            "message": "Connection timeout on external API",
            "timestamp": "2025-11-19T14:20:00.000000Z"
          },
          {
            "level": "ERROR",
            "message": "Failed to process payment order",
            "timestamp": "2025-11-19T14:18:00.000000Z"
          }
        ]
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
    },
    "help": {
      "system_health": {
        "domain_areas": {
          "what_it_is": "Distribution of users across different business domains (e-commerce, real estate, etc.)",
          "description": "This metric shows how many active users are registered in each domain area..."
        },
        "critical_alerts": {
          "what_it_is": "Recent error messages from application logs",
          "description": "This metric displays the last 5 ERROR-level messages found in your laravel.log file..."
        },
        "uptime": {
          "what_it_is": "System availability percentage over different time periods",
          "description": "This metric tracks how much time your system has been operational..."
        },
        "resource_utilization": {
          "what_it_is": "Server resource consumption (CPU, Memory, Disk)",
          "description": "This metric provides real-time information about your server's hardware resource usage..."
        }
      },
      "business_metrics": {
        "active_users": {
          "what_it_is": "Total number of registered users in the platform",
          "description": "This metric represents the cumulative count of all users..."
        },
        "new_users_month": {
          "what_it_is": "Number of users who registered during the current calendar month",
          "description": "This metric shows how many new users joined your platform..."
        },
        "growth_rate_mom": {
          "what_it_is": "Month-over-month user growth percentage",
          "description": "This metric calculates the percentage change in new user registrations..."
        },
        "revenue_total_cents": {
          "what_it_is": "Total revenue in cents since platform inception",
          "description": "Sum of all paid orders since platform inception..."
        },
        "revenue_month_cents": {
          "what_it_is": "Revenue generated in the current calendar month",
          "description": "Revenue generated in the current calendar month only..."
        },
        "platform_distribution": {
          "what_it_is": "How many platforms exist in each domain area",
          "description": "This metric shows the distribution of your platforms..."
        }
      },
      "quick_actions": {
        "deploy_status": {
          "what_it_is": "Current environment and version information",
          "description": "This metric provides technical details about your running environment..."
        },
        "pending_approvals": {
          "what_it_is": "Number of user role requests awaiting approval",
          "description": "This metric shows how many users have requested roles or permissions..."
        },
        "system_notifications": {
          "what_it_is": "Count of different log severity levels in your application",
          "description": "This metric analyzes your laravel.log file (last 500 lines)..."
        }
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

## Metrics Help

### System Health

#### Domain Areas
**What it is**: Distribution of users across different business domains (e-commerce, real estate, etc.)

**Help**: This metric shows how many active users are registered in each domain area of your platform ecosystem. Users are counted distinctly by their association through `platform_user_role`, meaning each unique user is counted only once per domain area, regardless of how many roles they have. This helps you understand which business areas are most actively used and where your user base is concentrated.

#### Critical Alerts
**What it is**: Recent error messages from application logs

**Help**: This metric displays the last 5 ERROR-level messages found in your `laravel.log` file. These are critical issues that occurred in your application and require attention. The system analyzes the last 500 lines of logs to find recent errors. Each alert includes the error level (ERROR, WARNING, CRITICAL, or EMERGENCY), the error message (truncated to 200 characters), and the timestamp when it occurred. Monitor this regularly to catch and resolve issues before they impact users.

#### Uptime
**What it is**: System availability percentage over different time periods

**Help**: This metric tracks how much time your system has been operational and accessible to users. It's measured as a percentage over three periods: last 24 hours, last 7 days, and last 30 days. Currently returns `null` until integrated with a monitoring system. A value of 99.9% means your system was down for only 0.1% of that time period. This is crucial for understanding your service reliability and meeting SLA commitments.

#### Resource Utilization
**What it is**: Server resource consumption (CPU, Memory, Disk)

**Help**: This metric provides real-time information about your server's hardware resource usage:
- **CPU**: Percentage of processing power being used. High values (>80%) may indicate performance bottlenecks.
- **Memory**: Shows used RAM versus available limit. Memory pressure can slow down your application or cause crashes.
- **Disk**: Storage space consumption. Running out of disk space can prevent logging, file uploads, and database operations.

These metrics help you identify when you need to scale resources or optimize your application.

### Business Metrics

#### Active Users
**What it is**: Total number of registered users in the platform

**Help**: This metric represents the cumulative count of all users who have ever registered on your platform across all domains. It's calculated by counting distinct users in the `platform_user_role` table. This is your total user base size and a key indicator of platform growth over time.

#### New Users (Month)
**What it is**: Number of users who registered during the current calendar month

**Help**: This metric shows how many new users joined your platform from the 1st day of the current month until now. It's calculated by counting users whose `created_at` date falls within the current month. Use this to track user acquisition effectiveness and identify trends in growth patterns.

#### Growth Rate (MoM)
**What it is**: Month-over-month user growth percentage

**Help**: This metric calculates the percentage change in new user registrations compared to the previous month. Formula: `((current_month - previous_month) / previous_month) × 100`. A positive value indicates growth, while a negative value means fewer users joined this month compared to last month (which can happen due to user deletions or seasonal variations). For example, 25% means you gained 25% more users this month than last month.

#### Revenue (Total & Monthly)
**What it is**: Total and current month's revenue in cents

**Help**: This metric tracks your platform's financial performance:
- **Total Revenue**: Sum of all paid orders since platform inception. Calculated from `payment_orders` table where `paid_at` is not null.
- **Monthly Revenue**: Revenue generated in the current calendar month only.

Values are in cents to avoid floating-point precision issues (e.g., 1500000000 cents = $15,000,000.00). This helps you monitor financial health and identify revenue trends.

#### Platform Distribution
**What it is**: How many platforms exist in each domain area

**Help**: This metric shows the distribution of your platforms across different business domains. Each entry shows the domain area name, a list of platform names in that domain, and the total count. This helps you understand your platform ecosystem structure and identify which domains have the most or least platform presence.

### Quick Actions

#### Deploy Status
**What it is**: Current environment and version information

**Help**: This metric provides technical details about your running environment:
- **Stage**: Whether you're in Production or Staging
- **Environment**: The Laravel environment name (production, staging, development)
- **Laravel Version**: The framework version you're running
- **PHP Version**: The PHP interpreter version

Use this to quickly verify you're looking at the correct environment and ensure you're running expected versions. Critical for troubleshooting and ensuring production stability.

#### Pending Approvals
**What it is**: Number of user role requests awaiting approval

**Help**: This metric shows how many users have requested roles or permissions that require administrative approval. These are records in `platform_user_role` with status 'requested'. Each pending approval represents a user waiting to gain access to specific platform features. High numbers may indicate a backlog in your approval process that could be frustrating users.

#### System Notifications
**What it is**: Count of different log severity levels in your application

**Help**: This metric analyzes your `laravel.log` file (last 500 lines) and categorizes issues by severity:
- **ERROR**: Application errors that need attention but didn't crash the system
- **WARNING**: Potentially problematic situations that should be investigated
- **CRITICAL**: Critical conditions that need immediate attention
- **EMERGENCY**: System is unusable, requires immediate action

The `latest_errors` array shows the 5 most recent ERROR-level messages with full details. Use this to monitor application health and prioritize which issues to address first. High error counts may indicate systemic problems requiring immediate investigation.

### Help Field in Response

The response now includes a `help` field containing descriptions for all metrics. Each metric has:
- `what_it_is`: A brief one-line summary of what the metric represents
- `description`: A detailed explanation of how the metric is calculated, what it means, and how to interpret it

This help information is included directly in the API response, making it easy for frontend applications to display contextual help to users without requiring additional API calls or hardcoded descriptions.

**Example usage in frontend:**
```javascript
// Display help tooltip when user hovers over a metric
const help = response.data.help.business_metrics.active_users;
tooltip.text = `${help.what_it_is}\n\n${help.description}`;
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
- **Users by Area**: DISTINCT count of `user_id` in `platform_user_role` grouped by `domain_area`
- **Critical Alerts**: Last 5 ERROR messages from the last 500 lines of `storage/logs/laravel.log`
- **System Notifications**: Analysis of last 500 lines from `storage/logs/laravel.log` searching for ERROR, WARNING, CRITICAL and EMERGENCY
- **Revenue**: Sum of `payment_orders` with non-null `paid_at`
- **Resources**: Server metrics via `sys_getloadavg()`, `memory_get_usage()`, `disk_free_space()`
- **Growth Rate**: Calculated as `((current_month_users - previous_month_users) / previous_month_users) * 100`, can be negative if users are deleted

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
- 2025-11-19: Changed `critical_alerts` to fetch ERRORs from laravel.log instead of unread messages
- 2025-11-19: Changed user count to use `platform_user_role` with DISTINCT `user_id`
- 2025-11-19: Growth rate can be negative if users are deleted
- 2025-11-19: Added `help` field in response with metric descriptions for frontend tooltips
