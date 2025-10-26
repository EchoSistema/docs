# ReputationBook – Backoffice Complaints Dashboard

## Description

Backoffice endpoint to retrieve complaint book dashboard metrics. Returns aggregated data with multiple time views (daily, weekly, bi-weekly, monthly, and yearly) that are mathematically consistent with each other.

## Endpoint

**GET** `/api/v1/backoffice/reputationbook/title/{title}`

## Authentication

Requires authentication via Bearer Token.

## Required Permissions

The authenticated user must have one of the following permissions:
- `show.all`
- `show.dashboards`

## URL Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| title | string | Yes | Title/identifier of the desired dashboard |

### Possible Values for `title`

- `general-metrics` - General complaint metrics
- Other custom titles as needed

## Success Response

**Code:** `200 OK`

### Response Structure

```json
{
  "data": {
    "_id": "ObjectId",
    "domain_area_id": 1,
    "platform_id": null,
    "company_id": null,
    "title": "general-metrics",
    "metrics": {
      "daily": {
        "period": {
          "type": "daily",
          "categories": ["00h", "01h", "02h", "03h", "04h", "05h", "06h", "07h", "08h", "09h", "10h", "11h", "12h", "13h", "14h", "15h", "16h", "17h", "18h", "19h", "20h", "21h", "22h", "23h"],
          "startDate": "2024-10-20",
          "endDate": "2024-10-20"
        },
        "summary": {
          "totalComplaints": 982,
          "averageHourlySolved": 14,
          "dailyChange": "+5%"
        },
        "statusBreakdown": {
          "solved": {
            "total": 350,
            "percentage": 35.6
          },
          "pending": {
            "total": 800,
            "percentage": 48.3
          },
          "unresolved": {
            "total": 320,
            "percentage": 12.6
          },
          "spam": {
            "total": 180,
            "percentage": 3.5
          }
        },
        "chartData": {
          "series": [
            {
              "name": "solved",
              "data": [8, 12, 15, 18, 22, 25, 28, 30, 32, 28, 25, 22, 20, 18, 15, 12, 10, 8, 6, 5, 4, 3, 2, 1]
            },
            {
              "name": "pending",
              "data": [25, 30, 35, 40, 42, 45, 48, 50, 52, 50, 48, 45, 42, 40, 38, 35, 32, 30, 28, 25, 22, 20, 18, 15]
            },
            {
              "name": "unresolved",
              "data": [10, 12, 14, 15, 16, 18, 20, 22, 24, 22, 20, 18, 16, 15, 14, 12, 10, 9, 8, 7, 6, 5, 4, 3]
            },
            {
              "name": "spam",
              "data": [5, 6, 7, 8, 9, 10, 11, 12, 13, 12, 11, 10, 9, 8, 7, 6, 5, 4, 3, 2, 2, 1, 1, 1]
            }
          ],
          "categories": ["00h", "01h", "02h", "03h", "04h", "05h", "06h", "07h", "08h", "09h", "10h", "11h", "12h", "13h", "14h", "15h", "16h", "17h", "18h", "19h", "20h", "21h", "22h", "23h"]
        }
      },
      "weekly": {
        "period": {
          "type": "week",
          "categories": ["Mon", "Tue", "Wed", "Thu", "Fri", "Sat", "Sun"],
          "startDate": "2024-10-20",
          "endDate": "2024-10-26"
        },
        "summary": {
          "totalComplaints": 11600,
          "averageDailySolved": 357,
          "weeklyChange": "+12%"
        },
        "statusBreakdown": {
          "solved": {
            "total": 2500,
            "percentage": 21.6
          },
          "pending": {
            "total": 5600,
            "percentage": 48.3
          },
          "unresolved": {
            "total": 2240,
            "percentage": 19.3
          },
          "spam": {
            "total": 1260,
            "percentage": 10.9
          }
        },
        "chartData": {
          "series": [
            {
              "name": "solved",
              "data": [350, 340, 360, 370, 355, 345, 380]
            },
            {
              "name": "pending",
              "data": [800, 780, 820, 810, 790, 770, 830]
            },
            {
              "name": "unresolved",
              "data": [320, 310, 330, 325, 315, 305, 335]
            },
            {
              "name": "spam",
              "data": [180, 170, 190, 185, 175, 165, 195]
            }
          ],
          "categories": ["Mon", "Tue", "Wed", "Thu", "Fri", "Sat", "Sun"]
        }
      },
      "bi_weekly": {
        "period": {
          "type": "bi_weekly",
          "categories": ["Week 1", "Week 2"],
          "startDate": "2024-10-20",
          "endDate": "2024-11-02"
        },
        "summary": {
          "totalComplaints": 23590,
          "averageWeeklySolved": 2535,
          "biWeeklyChange": "+10%"
        },
        "statusBreakdown": {
          "solved": {
            "total": 5070,
            "percentage": 21.5
          },
          "pending": {
            "total": 11360,
            "percentage": 48.1
          },
          "unresolved": {
            "total": 4560,
            "percentage": 19.3
          },
          "spam": {
            "total": 2600,
            "percentage": 11.0
          }
        },
        "chartData": {
          "series": [
            {
              "name": "solved",
              "data": [2500, 2570]
            },
            {
              "name": "pending",
              "data": [5600, 5760]
            },
            {
              "name": "unresolved",
              "data": [2240, 2320]
            },
            {
              "name": "spam",
              "data": [1260, 1340]
            }
          ],
          "categories": ["Week 1", "Week 2"]
        }
      },
      "monthly": {
        "period": {
          "type": "month",
          "categories": ["Week 1", "Week 2", "Week 3", "Week 4"],
          "startDate": "2024-10-01",
          "endDate": "2024-10-31"
        },
        "summary": {
          "totalComplaints": 46500,
          "averageWeeklySolved": 2500,
          "monthlyChange": "+8%"
        },
        "statusBreakdown": {
          "solved": {
            "total": 10000,
            "percentage": 21.5
          },
          "pending": {
            "total": 22400,
            "percentage": 48.2
          },
          "unresolved": {
            "total": 9000,
            "percentage": 19.4
          },
          "spam": {
            "total": 5100,
            "percentage": 11.0
          }
        },
        "chartData": {
          "series": [
            {
              "name": "solved",
              "data": [2500, 2570, 2450, 2480]
            },
            {
              "name": "pending",
              "data": [5600, 5760, 5500, 5540]
            },
            {
              "name": "unresolved",
              "data": [2240, 2320, 2200, 2240]
            },
            {
              "name": "spam",
              "data": [1260, 1340, 1230, 1270]
            }
          ],
          "categories": ["Week 1", "Week 2", "Week 3", "Week 4"]
        }
      },
      "yearly": {
        "period": {
          "type": "year",
          "categories": ["Jan", "Feb", "Mar", "Apr", "May", "Jun", "Jul", "Aug", "Sep", "Oct", "Nov", "Dec"],
          "startDate": "2024-01-01",
          "endDate": "2024-12-31"
        },
        "summary": {
          "totalComplaints": 241400,
          "averageMonthlySolved": 9083,
          "yearlyChange": "+15%"
        },
        "statusBreakdown": {
          "solved": {
            "total": 109000,
            "percentage": 45.2
          },
          "pending": {
            "total": 241400,
            "percentage": 48.2
          },
          "unresolved": {
            "total": 98500,
            "percentage": 19.4
          },
          "spam": {
            "total": 54300,
            "percentage": 11.0
          }
        },
        "chartData": {
          "series": [
            {
              "name": "solved",
              "data": [8200, 8400, 8600, 8800, 9000, 9200, 9100, 9000, 8900, 10000, 9500, 9800]
            },
            {
              "name": "pending",
              "data": [18400, 18800, 19200, 19600, 20000, 20400, 20200, 20000, 19800, 22400, 21000, 21600]
            },
            {
              "name": "unresolved",
              "data": [7400, 7600, 7800, 8000, 8200, 8400, 8300, 8200, 8100, 9000, 8600, 8900]
            },
            {
              "name": "spam",
              "data": [4200, 4300, 4400, 4500, 4600, 4700, 4650, 4600, 4550, 5100, 4800, 4900]
            }
          ],
          "categories": ["Jan", "Feb", "Mar", "Apr", "May", "Jun", "Jul", "Aug", "Sep", "Oct", "Nov", "Dec"]
        }
      }
    },
    "createdAt": "2024-10-20T00:00:00.000Z",
    "updatedAt": "2024-10-26T23:59:59.000Z"
  }
}
```

## Error Responses

### 404 Not Found - Domain Area not found

**Condition:** The "ReputationBook" domain area was not found in the system.

```json
{
  "message": "Domain area not found"
}
```

### 404 Not Found - Dashboard not found

**Condition:** No dashboard was found with the specified title for the ReputationBook area.

```json
{
  "message": "Dashboard not found"
}
```

### 403 Forbidden

**Condition:** User does not have the required permissions (`show.all` or `show.dashboards`).

```json
{
  "message": "You do not have permission to perform this action"
}
```

### 401 Unauthorized

**Condition:** Authentication token is missing or invalid.

```json
{
  "message": "Unauthenticated"
}
```

## Metrics Structure

### Mathematical Consistency

All time views are mathematically consistent:
- Sum of 24 hours = daily total
- Sum of 7 days = weekly total
- Sum of 2 weeks = bi-weekly total
- Sum of 4 weeks = monthly total
- October value in yearly = October monthly total

### Period Types

| Type | Granularity | Categories | Usage |
|------|-------------|------------|-------|
| `daily` | Hours (24h) | 00h-23h | Hourly analysis of a single day |
| `weekly` | Days (7d) | Mon-Sun | Daily analysis of the week |
| `bi_weekly` | Weeks (2) | Week 1-2 | Bi-weekly comparison |
| `monthly` | Weeks (4) | Week 1-4 | Weekly analysis of the month |
| `yearly` | Months (12) | Jan-Dec | Monthly analysis of the year |

### Complaint Statuses

| Status | Description |
|--------|-------------|
| `solved` | Resolved complaints (amicably or legally) |
| `pending` | Pending complaints (initiated, under review, approved, replied) |
| `unresolved` | Unresolved complaints |
| `spam` | Complaints marked as spam |

## Request Examples

### cURL

```bash
curl -X GET "https://api.example.com/api/v1/backoffice/reputationbook/title/general-metrics" \
  -H "Authorization: Bearer {your-token}" \
  -H "Accept: application/json"
```

### JavaScript (Fetch)

```javascript
fetch('https://api.example.com/api/v1/backoffice/reputationbook/title/general-metrics', {
  method: 'GET',
  headers: {
    'Authorization': 'Bearer {your-token}',
    'Accept': 'application/json'
  }
})
.then(response => response.json())
.then(data => console.log(data));
```

### PHP (Guzzle)

```php
use GuzzleHttp\Client;

$client = new Client();
$response = $client->get('https://api.example.com/api/v1/backoffice/reputationbook/title/general-metrics', [
    'headers' => [
        'Authorization' => 'Bearer {your-token}',
        'Accept' => 'application/json',
    ]
]);

$data = json_decode($response->getBody(), true);
```

## Use Cases

### 1. Administrative Dashboard

Display general complaint metrics with multiple time views for administrative analysis.

### 2. Executive Reports

Generate executive reports with aggregated data from different periods.

### 3. Interactive Charts

Create interactive charts (ApexCharts, Chart.js) using data from `chartData.series` and `chartData.categories`.

### 4. Performance KPIs

Monitor KPIs such as resolution rate, average time, and percentage variations between periods.

## Important Notes

1. **Permissions**: The endpoint checks if the user has permission `show.all` OR `show.dashboards`
2. **Domain Area**: The dashboard is linked to the "ReputationBook" domain area
3. **MongoDB**: Data is stored in MongoDB in the `dashboards` collection
4. **Cache**: Consider implementing caching to improve performance for frequent queries
5. **Pre-calculated Data**: All metrics are pre-calculated and stored, not computed in real-time

## Related

- [ComplaintBook Dashboard Metrics Structure](../Dashboards/ComplaintBook.md)
- [ComplaintBookDashboard Model](../Models/Mongo/ComplaintBookDashboard.md)
- [Backoffice Complaint Index](./BackofficeComplaintIndex.md)
- [Backoffice Complaint Show](./BackofficeComplaintShow.md)

## Changelog

- 2025-10-26: Initial endpoint documentation with coherent metrics structure
