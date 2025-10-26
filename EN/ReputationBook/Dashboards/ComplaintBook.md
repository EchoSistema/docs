# ReputationBook – Complaints Dashboard

## Description

This document describes the metrics structure stored in MongoDB for ReputationBook complaint dashboards. Data is stored in the `dashboards` collection of the `reputation-book` database.

## Model

**Namespace**: `Domain\ReputationBook\Models\Mongo\ComplaintBookDashboard`

**Collection**: `dashboards`

**Database**: `reputation-book`

## `metrics` Field Structure

The `metrics` field is a JSON object containing different types of metrics grouped by categories. Each metric has its own data structure.

---

## Available Metrics

### 1. Complaints by Status (`complaint_by_status`)

Metric showing the distribution of complaints by status over a specific period.

#### Structure

```json
{
  "complaint_by_status": {
    "period": {
      "type": "week",
      "categories": ["Mon", "Tue", "Wed", "Thu", "Fri", "Sat", "Sun"],
      "startDate": "2024-10-20",
      "endDate": "2024-10-26"
    },
    "summary": {
      "totalComplaints": 7300,
      "averageWeeklySolved": 1500,
      "weeklyChange": "+12%"
    },
    "statusBreakdown": {
      "solved": {
        "total": 1500,
        "percentage": 20.5
      },
      "pending": {
        "total": 4700,
        "percentage": 64.4
      },
      "unresolved": {
        "total": 780,
        "percentage": 10.7
      },
      "spam": {
        "total": 320,
        "percentage": 4.4
      }
    },
    "chartData": {
      "series": [
        {
          "name": "solved",
          "data": [28, 50, 90, 95, 20, 70, 35]
        },
        {
          "name": "pending",
          "data": [80, 60, 70, 30, 45, 20, 80]
        },
        {
          "name": "unresolved",
          "data": [32, 23, 78, 35, 65, 35, 15]
        },
        {
          "name": "spam",
          "data": [60, 25, 80, 25, 15, 40, 15]
        }
      ],
      "categories": ["Mon", "Tue", "Wed", "Thu", "Fri", "Sat", "Sun"]
    }
  }
}
```

#### Detailed Fields

##### `period` - Period Information

| Field | Type | Description |
| ----- | ---- | ----------- |
| type | string | Period type (week, month, year, custom) |
| categories | array | Category/date labels for the period |
| startDate | string | Period start date (format: YYYY-MM-DD) |
| endDate | string | Period end date (format: YYYY-MM-DD) |

**Possible values for `type`**:
- `week` - Weekly period
- `month` - Monthly period
- `year` - Yearly period
- `custom` - Custom period

##### `summary` - General Summary

| Field | Type | Description |
| ----- | ---- | ----------- |
| totalComplaints | integer | Total complaints in the period |
| averageWeeklySolved | integer | Average complaints solved per week |
| weeklyChange | string | Percentage variation from previous week (e.g., "+12%", "-5%") |

##### `statusBreakdown` - Status Distribution

Object containing count and percentage for each complaint status.

**Available statuses**:

| Status | Field | Description |
| ------ | ----- | ----------- |
| Solved | solved | Resolved complaints |
| Pending | pending | Complaints awaiting response/action |
| Unresolved | unresolved | Complaints that were not resolved |
| Spam | spam | Complaints marked as spam |

**Structure for each status**:

| Field | Type | Description |
| ----- | ---- | ----------- |
| total | integer | Total complaints in this status |
| percentage | float | Percentage relative to total |

##### `chartData` - Chart Data

Structure prepared for chart visualization (compatible with ApexCharts, Chart.js, etc).

| Field | Type | Description |
| ----- | ---- | ----------- |
| series | array | Array of data series |
| series[].name | string | Series name (status) in lowercase |
| series[].data | array | Array of numeric values corresponding to categories |
| categories | array | X-axis labels (days, months, etc) |

---

## Period Types

### Weekly (`week`)
- **categories**: `["Mon", "Tue", "Wed", "Thu", "Fri", "Sat", "Sun"]`
- **Duration**: 7 days
- **Usage**: Daily analysis of the week

### Monthly (`month`)
- **categories**: `["Week 1", "Week 2", "Week 3", "Week 4"]` or `["1", "2", ..., "31"]`
- **Duration**: 1 month
- **Usage**: Weekly or daily analysis of the month

### Yearly (`year`)
- **categories**: `["Jan", "Feb", "Mar", ..., "Dec"]`
- **Duration**: 12 months
- **Usage**: Monthly analysis of the year

### Custom (`custom`)
- **categories**: User-defined
- **Duration**: Custom period
- **Usage**: Specific analyses with custom dates

---

## Complaint Statuses

### Lifecycle

1. **Initiated** - Complaint initiated
2. **Pending** - Awaiting review/response
3. **Approved** - Approved for publication
4. **Replied** - Company responded
5. **Solved** (Amicably/Legally) - Resolved
6. **Unresolved** - Not resolved
7. **Spam** - Marked as spam
8. **Archived** - Archived
9. **Canceled** - Canceled

### Dashboard Grouping

- **Solved**: `amicably_resolved`, `legally_resolved`
- **Pending**: `initiated`, `pending`, `approved`, `replied`
- **Unresolved**: `unresolved`
- **Spam**: `spam`

---

## Calculations and Formulas

### Percentage by Status

```
percentage = (total_status / total_complaints) * 100
```

### Weekly Variation

```
weeklyChange = ((current_week - previous_week) / previous_week) * 100
```

Example:
- Previous Week: 1340 solved
- Current Week: 1500 solved
- Variation: `((1500 - 1340) / 1340) * 100 = +11.94%` ≈ `+12%`

### Weekly Average

```
averageWeeklySolved = sum(solved_per_week) / total_weeks
```

---

## Use Cases

### 1. Stacked Area Chart

Shows the evolution of each status over time in a stacked manner.

**Data**: `chartData.series`

**X-Axis**: `chartData.categories`

**Y-Axis**: Number of complaints

### 2. Pie/Donut Chart

Shows current distribution by status.

**Data**: `statusBreakdown` (use `total` or `percentage` values)

**Labels**: Status names (solved, pending, unresolved, spam)

### 3. Summary Cards

Display key metrics in dashboard cards.

**Data**: `summary`
- Total Complaints
- Average Weekly Solved
- Week Variation

### 4. Status Table

Detailed list of each status with total and percentage.

**Data**: `statusBreakdown`

---

## MongoDB Query Examples

### Find Current Week Dashboard

```javascript
db.dashboards.findOne({
  "metrics.complaint_by_status.period.type": "week",
  "metrics.complaint_by_status.period.startDate": "2024-10-20",
  "metrics.complaint_by_status.period.endDate": "2024-10-26"
})
```

### Find Dashboard by Platform

```javascript
db.dashboards.findOne({
  "platform_id": 1,
  "period": "2024-10-20_2024-10-26"
})
```

### Multi-Platform Aggregation

```javascript
db.dashboards.aggregate([
  {
    $match: {
      "metrics.complaint_by_status.period.type": "week",
      "createdAt": { $gte: ISODate("2024-10-20") }
    }
  },
  {
    $group: {
      _id: "$metrics.complaint_by_status.period.type",
      totalComplaints: { $sum: "$metrics.complaint_by_status.summary.totalComplaints" },
      totalSolved: { $sum: "$metrics.complaint_by_status.statusBreakdown.solved.total" }
    }
  }
])
```

---

## Best Practices

### 1. Data Updates
- Update metrics daily via cron/job
- Maintain history of previous periods
- Use `updateOrCreate` to avoid duplicates

### 2. Performance
- Index by `platform_id`, `company_id`, and `period`
- Store pre-calculated data
- Avoid real-time calculations on frontend

### 3. Versioning
- Include `version` field for structure migration
- Maintain backward compatibility
- Document structure changes

### 4. Validation
- Validate that `categories` and `series[].data` have same length
- Ensure percentages sum to ~100%
- Verify period start/end dates

---

## Complete Document Structure

```json
{
  "_id": "ObjectId",
  "platform_id": 1,
  "company_id": null,
  "period": "2024-10-20_2024-10-26",
  "metrics": {
    "complaint_by_status": {
      "period": { ... },
      "summary": { ... },
      "statusBreakdown": { ... },
      "chartData": { ... }
    },
    "complaint_by_company": { ... },
    "complaint_resolution_time": { ... },
    "complaint_satisfaction": { ... }
  },
  "createdAt": "2024-10-20T00:00:00.000Z",
  "updatedAt": "2024-10-26T23:59:59.000Z"
}
```

---

## Future Metrics (Planned)

### `complaint_by_company`
Distribution of complaints by company in the period.

### `complaint_resolution_time`
Average complaint resolution time by status.

### `complaint_satisfaction`
User satisfaction index with resolutions.

### `complaint_by_category`
Distribution of complaints by product/service category.

### `complaint_by_platform`
Comparative analysis across different platforms.

---

## Related

- [ComplaintBookDashboard Model](../Models/Mongo/ComplaintBookDashboard.md)
- [Complaint Model](../Models/Complaint.md)
- [Dashboard API Endpoints](../Endpoints/BackofficeComplaintBookDashboard.md)

## Changelog

- 2025-10-26: Initial documentation with `complaint_by_status` structure
