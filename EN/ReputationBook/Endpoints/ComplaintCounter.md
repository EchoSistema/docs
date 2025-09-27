# ReputationBook – Complaint Metrics

## Counter Summary

### Endpoint

```
GET /api/v1/complaint-book/complaints/counter
```

Returns the total number of complaints and the distribution per status based on the filters provided.

### Authentication

Required – `Bearer {token}` with abilities `backoffice` and `domain:reputationbook`.

### Query Parameters

Accepts the same filters as `ComplaintFilterRequest` (status, company, time ranges, etc.).

### Example Response (200)

```json
{
  "data": {
    "total": 128,
    "status": {
      "open": 64,
      "in_progress": 40,
      "resolved": 24
    },
    "month": 12
  }
}
```

> The `month` key is returned only when the `month` filter is provided.

### HTTP Status Codes

- 200: Data returned.
- 401/403: Missing permissions.

---

## Interval Report

### Endpoint

```
GET /api/v1/complaint-book/complaints/report/{interval}
```

Generates a time series based on the latest status changes.

### Path Parameters

| Parameter | Accepted Values |
| --------- | ---------------- |
| `interval` | `annual`, `semi-annual`, `tri-monthly`, `bi-monthly`, `monthly`, `weekly` (any other value defaults to the current year). |

### Response

```json
{
  "success": true,
  "counter": {
    "2025-06": {
      "open": 12,
      "resolved": 4
    },
    "2025-07": {
      "open": 8,
      "in_progress": 6,
      "resolved": 9
    }
  },
  "interval": 6
}
```

- `counter`: map `YYYY-MM` → totals per status (only the most recent event per complaint within the period is counted).
- `interval`: number of months covered by the selected range.

### HTTP Status Codes

- 200: Report generated.
- 401/403: Missing permissions.

