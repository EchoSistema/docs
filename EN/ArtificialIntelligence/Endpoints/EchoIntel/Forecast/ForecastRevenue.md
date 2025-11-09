# Artificial Intelligence – Revenue Forecasting

## Endpoint

```
POST /api/v1/ai/echointel/forecast/revenue
```

Performs forecasting of future revenue using time series analysis and machine learning models. The system employs a model zoo approach, testing multiple algorithms to select the champion model with the best performance.

## Authentication

Required – Bearer {token} with middleware `auth:sanctum`

## Headers

| Header          | Type   | Required | Description |
| ------------------ | ------ | ----------- | --------- |
| Authorization      | string | Yes         | `Bearer {token}`. |
| X-Customer-Api-Id  | string | Conditional | Tenant UUID (v4). |
| X-Secret           | string | Conditional | 64-character secret. |
| Accept-Language    | string | No         | Language (`en`, `es`, `pt`). |
| Content-Type       | string | Yes         | `application/json`. |

## Parameters

### Request Body Parameters

| Parameter          | Type   | Required | Description | Default | Constraints |
| ------------------ | ------ | ----------- | --------- | ------- | ----------- |
| historical_revenue | array  | Yes         | Historical revenue data. Each element must contain `date` and `revenue` fields. | - | Minimum 60 data points (approximately 2 months of daily data) |
| forecast_periods   | int    | Yes         | Number of future periods to forecast. | - | Range: 1-365 days |
| granularity        | string | No         | Temporal granularity for forecasting. | `monthly` | Values: `daily`, `weekly`, `monthly` |
| confidence_level   | float  | No         | Confidence level for prediction intervals. | `0.95` | Range: 0.80-0.99 |
| external_factors   | object | No         | External factors affecting revenue (seasonality, promotions, holidays, etc.). | `null` | Optional object with custom factors |

> **Note:** Parameters accept both `snake_case` and `camelCase` formats.

## Examples

### Request Example (curl)

```bash
curl -X POST \
  -H "Authorization: Bearer <token>" \
  -H "X-Customer-Api-Id: <tenant-uuid>" \
  -H "X-Secret: <secret>" \
  -H "Content-Type: application/json" \
  -d '{
    "historical_revenue": [
      {"date": "2024-01", "revenue": 150000},
      {"date": "2024-02", "revenue": 165000},
      {"date": "2024-03", "revenue": 172000},
      {"date": "2024-04", "revenue": 168000},
      {"date": "2024-05", "revenue": 180000},
      {"date": "2024-06", "revenue": 195000}
    ],
    "forecast_periods": 6,
    "granularity": "monthly",
    "confidence_level": 0.95
  }' \
  "https://echosistema.online/api/v1/ai/echointel/forecast/revenue"
```

## Response

### Success `200 OK`

The API returns forecasts in two distinct views: calendar view (all days) and business days view (working days only).

```json
{
  "forecast_calendar": [
    {
      "period": "2025-01-01",
      "predicted_revenue": 205000,
      "lower_bound": 192000,
      "upper_bound": 218000,
      "confidence": 0.95
    },
    {
      "period": "2025-01-02",
      "predicted_revenue": 212000,
      "lower_bound": 197500,
      "upper_bound": 226500,
      "confidence": 0.95
    },
    {
      "period": "2025-01-03",
      "predicted_revenue": 218000,
      "lower_bound": 202000,
      "upper_bound": 234000,
      "confidence": 0.95
    }
  ],
  "forecast_business_days": [
    {
      "period": "2025-01-02",
      "predicted_revenue": 212000,
      "lower_bound": 197500,
      "upper_bound": 226500,
      "confidence": 0.95,
      "is_business_day": true
    },
    {
      "period": "2025-01-03",
      "predicted_revenue": 218000,
      "lower_bound": 202000,
      "upper_bound": 234000,
      "confidence": 0.95,
      "is_business_day": true
    }
  ],
  "total_forecast": {
    "sum": 1272000,
    "avg": 212000,
    "growth_rate": 0.085
  },
  "champion_model": "Prophet",
  "model_metrics": {
    "calendar_rmse": 5890.3,
    "business_days_rmse": 4250.5,
    "weighted_rmse": 5278.12,
    "mae": 4250.5,
    "mape": 0.029,
    "r_squared": 0.94
  },
  "preprocessing_applied": {
    "outlier_detection": true,
    "box_cox_transformation": true,
    "missing_value_imputation": false,
    "decomposition": "additive"
  },
  "cross_validation_scores": {
    "folds": 4,
    "method": "rolling_origin",
    "avg_rmse": 5278.12,
    "avg_mae": 4250.5
  },
  "models_tested": [
    {
      "name": "ARIMA",
      "weighted_rmse": 5890.3,
      "rank": 2
    },
    {
      "name": "Prophet",
      "weighted_rmse": 5278.12,
      "rank": 1
    },
    {
      "name": "ETS",
      "weighted_rmse": 6120.8,
      "rank": 3
    },
    {
      "name": "TBATS",
      "weighted_rmse": 6350.2,
      "rank": 4
    },
    {
      "name": "Croston",
      "weighted_rmse": 7250.5,
      "rank": 5
    },
    {
      "name": "Ensemble",
      "weighted_rmse": 5450.7,
      "rank": 2
    }
  ],
  "performance": {
    "processing_time_ms": 320,
    "data_points_used": 90
  }
}
```

## Explained JSON Structure

| Field                                   | Type    | Description |
| --------------------------------------- | ------- | --------- |
| `forecast_calendar`                     | array   | Forecast for all calendar days (including weekends and holidays). |
| `forecast_calendar[].period`            | string  | ISO 8601 date for the forecast period. |
| `forecast_calendar[].predicted_revenue` | float   | Predicted revenue for the period. |
| `forecast_calendar[].lower_bound`       | float   | Lower bound of the confidence interval. |
| `forecast_calendar[].upper_bound`       | float   | Upper bound of the confidence interval. |
| `forecast_calendar[].confidence`        | float   | Confidence level (e.g., 0.95 for 95%). |
| `forecast_business_days`                | array   | Forecast for business days only (excludes weekends/holidays). |
| `forecast_business_days[].period`       | string  | ISO 8601 date for the business day. |
| `forecast_business_days[].predicted_revenue` | float | Predicted revenue for the business day. |
| `forecast_business_days[].lower_bound`  | float   | Lower bound of the confidence interval. |
| `forecast_business_days[].upper_bound`  | float   | Upper bound of the confidence interval. |
| `forecast_business_days[].confidence`   | float   | Confidence level. |
| `forecast_business_days[].is_business_day` | boolean | Always true for this array. |
| `total_forecast`                        | object  | Aggregate metrics across the forecast period. |
| `total_forecast.sum`                    | float   | Total predicted revenue sum. |
| `total_forecast.avg`                    | float   | Average predicted revenue per period. |
| `total_forecast.growth_rate`            | float   | Estimated growth rate compared to historical data. |
| `champion_model`                        | string  | Name of the selected champion model (e.g., ARIMA, Prophet, ETS, TBATS, Croston, Ensemble). |
| `model_metrics`                         | object  | Performance metrics of the champion model. |
| `model_metrics.calendar_rmse`           | float   | Root Mean Squared Error for calendar view. |
| `model_metrics.business_days_rmse`      | float   | Root Mean Squared Error for business days view. |
| `model_metrics.weighted_rmse`           | float   | Weighted RMSE (calendar_rmse * 0.6 + business_days_rmse * 0.4). |
| `model_metrics.mae`                     | float   | Mean Absolute Error. |
| `model_metrics.mape`                    | float   | Mean Absolute Percentage Error (0-1 scale). |
| `model_metrics.r_squared`               | float   | R-squared coefficient of determination (0-1). |
| `preprocessing_applied`                 | object  | Pre-processing steps applied to the data. |
| `preprocessing_applied.outlier_detection` | boolean | Whether outlier detection via IQR was applied. |
| `preprocessing_applied.box_cox_transformation` | boolean | Whether Box-Cox transformation was applied for variance stabilization. |
| `preprocessing_applied.missing_value_imputation` | boolean | Whether missing values were imputed. |
| `preprocessing_applied.decomposition`   | string  | Type of decomposition used (additive, multiplicative, or none). |
| `cross_validation_scores`               | object  | Cross-validation performance metrics. |
| `cross_validation_scores.folds`         | int     | Number of cross-validation folds (typically 4). |
| `cross_validation_scores.method`        | string  | Cross-validation method used (rolling_origin). |
| `cross_validation_scores.avg_rmse`      | float   | Average RMSE across all folds. |
| `cross_validation_scores.avg_mae`       | float   | Average MAE across all folds. |
| `models_tested`                         | array   | List of all models tested in the model zoo. |
| `models_tested[].name`                  | string  | Name of the model (ARIMA, Prophet, ETS, TBATS, Croston, Ensemble). |
| `models_tested[].weighted_rmse`         | float   | Weighted RMSE score for the model. |
| `models_tested[].rank`                  | int     | Ranking of the model (1 = best). |
| `performance`                           | object  | Performance metadata. |
| `performance.processing_time_ms`        | int     | Processing time in milliseconds (typical: ~300ms for 90 days). |
| `performance.data_points_used`          | int     | Number of historical data points used in the forecast. |

## HTTP Status

| Status Code | Description |
| ----------- | ----------- |
| `200 OK` | Forecast generated successfully. Response includes dual-view forecasts and model metrics. |
| `400 Bad Request` | Invalid request parameters (e.g., missing required fields, invalid data types). |
| `401 Unauthorized` | Missing or invalid authentication token. |
| `403 Forbidden` | Valid token but insufficient permissions or invalid tenant credentials. |
| `422 Unprocessable Entity` | Request validation failed (e.g., insufficient data points, invalid date formats, non-numeric revenue values). |
| `429 Too Many Requests` | Rate limit exceeded. Retry after the specified time. |
| `500 Internal Server Error` | Server-side error during forecast processing. |
| `503 Service Unavailable` | EchoIntel service temporarily unavailable. |

## Errors

### Error Response Format

```json
{
  "error": "Validation Error",
  "message": "Insufficient historical data points",
  "details": {
    "required_minimum": 60,
    "provided": 45,
    "recommendation": "Provide at least 60 data points (approximately 2 months of daily data) for accurate forecasting."
  }
}
```

### Common Error Conditions

| Error | HTTP Status | Cause | Solution |
| ----- | ----------- | ----- | -------- |
| Insufficient data points | `422` | Less than 60 historical data points provided. | Provide at least 60 rows of historical revenue data. |
| All values are zeros | `422` | All revenue values in historical data are 0. | Ensure historical data contains non-zero revenue values. |
| Excessive missing values | `422` | More than 80% of values are missing or null. | Reduce missing values to less than 80% of the dataset. |
| Non-numeric revenue values | `422` | Revenue field contains non-numeric data (strings, nulls, etc.). | Ensure all revenue values are valid numbers. |
| Invalid date formats | `422` | Date field does not follow ISO 8601 or recognized formats. | Use ISO 8601 format (YYYY-MM-DD) for dates. |
| Invalid granularity | `400` | Granularity value is not one of: daily, weekly, monthly. | Use valid granularity: `daily`, `weekly`, or `monthly`. |
| Invalid confidence level | `400` | Confidence level outside range 0.80-0.99. | Provide confidence level between 0.80 and 0.99. |
| Forecast periods out of range | `400` | Forecast periods exceed maximum of 365. | Request forecast for 1-365 periods. |
| Service timeout | `503` | Processing took longer than allowed timeout. | Reduce the forecast period or simplify external factors. |

### Error Example: Insufficient Data

```json
{
  "error": "Validation Error",
  "message": "Insufficient historical data points",
  "details": {
    "required_minimum": 60,
    "provided": 45,
    "recommendation": "Provide at least 60 data points (approximately 2 months of daily data) for accurate forecasting."
  },
  "code": "INSUFFICIENT_DATA"
}
```

### Error Example: Invalid Date Format

```json
{
  "error": "Validation Error",
  "message": "Invalid date format in historical_revenue",
  "details": {
    "invalid_dates": ["2024/01/15", "15-01-2024"],
    "expected_format": "YYYY-MM-DD (ISO 8601)",
    "recommendation": "Use ISO 8601 date format: YYYY-MM-DD"
  },
  "code": "INVALID_DATE_FORMAT"
}
```

## How It Is Computed

### Model Zoo Approach

The revenue forecasting system employs a **model zoo** methodology, testing six different forecasting algorithms to identify the champion model:

#### 1. ARIMA (Auto-Regressive Integrated Moving Average)
- **Use case:** Time series with trends and autocorrelation
- **Parameters:** Auto-selected using AIC (Akaike Information Criterion)
- **Strengths:** Excellent for stationary data with clear patterns
- **Weaknesses:** Struggles with multiple seasonality

#### 2. Prophet (Facebook's Time Series Forecasting)
- **Use case:** Time series with strong seasonality and holidays
- **Parameters:** Automatic seasonality detection, trend changepoints
- **Strengths:** Handles missing data well, robust to outliers
- **Weaknesses:** Can overfit on short time series

#### 3. ETS (Exponential Smoothing)
- **Use case:** Time series with trend and/or seasonality
- **Parameters:** Error, Trend, Seasonal components auto-selected
- **Strengths:** Fast, interpretable, good for short-term forecasts
- **Weaknesses:** Less accurate for long-term forecasts

#### 4. TBATS (Trigonometric Seasonality, Box-Cox transformation, ARMA errors, Trend, Seasonal)
- **Use case:** Complex seasonality patterns (multiple cycles)
- **Parameters:** Automatic Box-Cox transformation, Fourier terms
- **Strengths:** Handles multiple seasonal periods
- **Weaknesses:** Computationally intensive

#### 5. Croston's Method
- **Use case:** Intermittent demand forecasting
- **Parameters:** Smoothing parameter alpha
- **Strengths:** Specialized for sparse data
- **Weaknesses:** Not suitable for continuous revenue streams

#### 6. Ensemble (Weighted Average)
- **Use case:** Combining predictions from multiple models
- **Parameters:** Weights based on individual model performance
- **Strengths:** Reduces variance, improves robustness
- **Weaknesses:** Can be conservative

### Champion Selection Logic

The champion model is selected based on the **lowest weighted RMSE** across both calendar and business day views:

```
weighted_rmse = (calendar_rmse × 0.6) + (business_days_rmse × 0.4)
```

**Rationale:**
- Calendar view (60% weight): Represents the complete picture
- Business days view (40% weight): Focuses on operational days
- The model with the lowest weighted RMSE becomes the champion

### Pre-processing Pipeline

Before model training, the following pre-processing steps are applied:

1. **Outlier Detection via IQR (Interquartile Range)**
   - Identifies values outside Q1 - 1.5×IQR and Q3 + 1.5×IQR
   - Outliers are capped or removed based on severity

2. **Box-Cox Transformation**
   - Stabilizes variance across the time series
   - Automatically determines the optimal lambda parameter
   - Applied when heteroscedasticity is detected

3. **Missing Value Imputation**
   - Forward fill for short gaps (1-2 periods)
   - Linear interpolation for medium gaps (3-7 periods)
   - Seasonal average for longer gaps

4. **Trend and Seasonality Decomposition**
   - Separates time series into trend, seasonal, and residual components
   - Uses STL decomposition (Seasonal and Trend decomposition using Loess)
   - Determines additive vs. multiplicative decomposition

### Cross-Validation Strategy

The system uses **rolling origin cross-validation** with walk-forward validation:

- **Folds:** 4
- **Method:** Time series split maintaining temporal order
- **Validation:** Each fold trains on historical data and validates on the next period
- **Metrics:** RMSE and MAE averaged across all folds

### Dual-View Forecasting

The API returns two distinct forecast arrays:

1. **Calendar View (`forecast_calendar`)**
   - Includes all days (weekends, holidays, working days)
   - Useful for financial planning and budgeting
   - Accounts for all temporal periods

2. **Business Days View (`forecast_business_days`)**
   - Excludes weekends and holidays
   - Useful for operational planning
   - Focuses on revenue-generating days

### Performance Optimization

- **Typical latency:** ~300-400ms for 90 days of historical data
- **Caching:** Model parameters cached for similar datasets
- **Parallel processing:** Models trained concurrently in the zoo
- **Early stopping:** Poor-performing models terminated early

## Notes

- **MAPE Interpretation:** Values < 0.10 (10%) are considered excellent, 0.10-0.20 good, 0.20-0.50 acceptable, > 0.50 poor.
- **R² Interpretation:** Values close to 1 indicate better model fit. R² > 0.90 is excellent for revenue forecasting.
- **Confidence Intervals:** Wider intervals indicate higher forecast uncertainty. Consider using lower forecast periods for narrower intervals.
- **Minimum Data Requirements:** At least 60 data points (approximately 2 months of daily data) required for reliable forecasting.
- **Maximum Forecast Horizon:** 365 days. Longer forecasts may have reduced accuracy.
- **External Factors:** Optional parameter that can include seasonality adjustments, promotional impacts, or economic indicators.
- **Granularity Impact:** Daily granularity requires more data points and may have higher variance than monthly granularity.

## Typical Workflow

### Step 1: Prepare Historical Data

Collect at least 60 data points of historical revenue:

```json
{
  "historical_revenue": [
    {"date": "2024-01-01", "revenue": 150000},
    {"date": "2024-01-02", "revenue": 165000},
    ...
    {"date": "2024-03-31", "revenue": 195000}
  ]
}
```

### Step 2: Define Forecast Parameters

Specify the number of periods to forecast and granularity:

```json
{
  "forecast_periods": 30,
  "granularity": "daily",
  "confidence_level": 0.95
}
```

### Step 3: Make API Request

Send POST request to the endpoint with authentication:

```bash
curl -X POST https://echosistema.online/api/v1/ai/echointel/forecast/revenue \
  -H "Authorization: Bearer {token}" \
  -H "X-Customer-Api-Id: {tenant-uuid}" \
  -H "Content-Type: application/json" \
  -d @request.json
```

### Step 4: Analyze Dual-View Forecasts

Review both calendar and business day forecasts:
- Use `forecast_calendar` for budgeting and financial planning
- Use `forecast_business_days` for operational capacity planning

### Step 5: Evaluate Model Performance

Check the champion model and metrics:
- `champion_model`: Which algorithm performed best
- `weighted_rmse`: Overall prediction accuracy
- `cross_validation_scores`: Validation performance

### Step 6: Understand Preprocessing

Review `preprocessing_applied` to understand data transformations:
- Outlier detection and handling
- Variance stabilization
- Missing value treatment

### Step 7: Incorporate into Planning

Use forecast data for:
- Revenue projections
- Resource allocation
- Budget planning
- Capacity management

## Related

- [Forecast Units](./ForecastUnits.md) - Unit-based forecasting for inventory planning
- [Forecast Units Asyncio](./ForecastUnitsAsyncio.md) - Asynchronous unit forecasting for high-volume data
- [Forecast Cost](./ForecastCost.md) - Cost forecasting using time series analysis
- [Forecast Cost Improved](./ForecastCostImproved.md) - Enhanced cost forecasting with advanced algorithms
- [Customer CLV Forecast](../CustomerIntelligence/CustomerClvForecast.md) - Customer Lifetime Value prediction

## References

* Controller: `src/Domain/ArtificialIntelligence/Http/Controllers/EchoIntelProxyController.php:158`
* Endpoint Method: `forecastRevenue(Request $request): JsonResponse`
* Route: `POST /api/v1/ai/echointel/forecast/revenue`
