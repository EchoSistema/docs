# Artificial Intelligence – Revenue Forecasting

## Endpoint

```
POST /api/v1/ai/echointel/forecast/revenue
```

Performs forecasting of future revenue using time series analysis y machine learning models. The system employs a model zoo approach, testing multiple algorithms to select the champion model with the best performance.

## Autenticación

Requerido – Bearer {token} con middleware `auth:sanctum`

## Encabezados

| Encabezado          | Tipo   | Requerido | Descripción |
| ------------------ | ------ | ----------- | --------- |
| Authorization      | string | Sí         | `Bearer {token}`. |
| X-Customer-Api-Id  | string | Condicional | UUID del tenant (v4). |
| X-Secret           | string | Condicional | 64-caracteres de secreto. |
| Accept-Language    | string | No         | Language (`en`, `es`, `pt`). |
| Content-Tipo       | string | Sí         | `application/json`. |

## Parámetros

### Cuerpo de la Solicitud Parámetros

| Parámetro          | Tipo   | Requerido | Descripción | Por Defecto | Constraints |
| ------------------ | ------ | ----------- | --------- | ------- | ----------- |
| historical_revenue | array  | Sí         | Historical revenue data. Each element must contain `date` y `revenue` fields. | - | Minimum 60 data points (approximately 2 meses of daily data) |
| forecast_periods   | int    | Sí         | Number of future periods to forecast. | - | Range: 1-365 días |
| granularity        | string | No         | Temporal granularity for forecasting. | `monthly` | Values: `daily`, `weekly`, `monthly` |
| confidence_level   | float  | No         | Confidence level for prediction intervals. | `0.95` | Range: 0.80-0.99 |
| external_factors   | object | No         | External factors affecting revenue (seasonality, promotions, holidías, etc.). | `null` | Optional object with custom factors |

> **Note:** Los parámetros aceptan tanto `snake_case` y `camelCase` formats.

## Ejemplos

### Ejemplo de Solicitud (curl)

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

## Respuesta

### Éxito `200 OK`

The API returns forecasts in two distinct views: calendar view (all días) y business días view (working días only).

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

## Explained Estructura JSON

| Campo                                   | Tipo    | Descripción |
| --------------------------------------- | ------- | --------- |
| `forecast_calendar`                     | array   | Forecast for all calendar días (including weekends y holidías). |
| `forecast_calendar[].period`            | string  | ISO 8601 date for the forecast period. |
| `forecast_calendar[].predicted_revenue` | float   | Predicted revenue for the period. |
| `forecast_calendar[].lower_bound`       | float   | Lower bound of the confidence interval. |
| `forecast_calendar[].upper_bound`       | float   | Upper bound of the confidence interval. |
| `forecast_calendar[].confidence`        | float   | Confidence level (e.g., 0.95 for 95%). |
| `forecast_business_días`                | array   | Forecast for business días only (excludes weekends/holidías). |
| `forecast_business_días[].period`       | string  | ISO 8601 date for the business day. |
| `forecast_business_días[].predicted_revenue` | float | Predicted revenue for the business day. |
| `forecast_business_días[].lower_bound`  | float   | Lower bound of the confidence interval. |
| `forecast_business_días[].upper_bound`  | float   | Upper bound of the confidence interval. |
| `forecast_business_días[].confidence`   | float   | Confidence level. |
| `forecast_business_días[].is_business_day` | boolean | Always true for this array. |
| `total_forecast`                        | object  | Aggregate metrics across the forecast period. |
| `total_forecast.sum`                    | float   | Total predicted revenue sum. |
| `total_forecast.avg`                    | float   | Average predicted revenue per period. |
| `total_forecast.growth_rate`            | float   | Estimated growth rate compared to historical data. |
| `champion_model`                        | string  | Name of the selected champion model (e.g., ARIMA, Prophet, ETS, TBATS, Croston, Ensemble). |
| `model_metrics`                         | object  | Rendimiento metrics of the champion model. |
| `model_metrics.calendar_rmse`           | float   | Root Mean Squared Error for calendar view. |
| `model_metrics.business_días_rmse`      | float   | Root Mean Squared Error for business días view. |
| `model_metrics.weighted_rmse`           | float   | Weighted RMSE (calendar_rmse * 0.6 + business_días_rmse * 0.4). |
| `model_metrics.mae`                     | float   | Mean Absolute Error. |
| `model_metrics.mape`                    | float   | Mean Absolute Percentage Error (0-1 scale). |
| `model_metrics.r_squared`               | float   | R-squared coefficient of determination (0-1). |
| `preprocessing_applied`                 | object  | Pre-processing steps applied to the data. |
| `preprocessing_applied.outlier_detection` | boolean | Whether outlier detection via IQR was applied. |
| `preprocessing_applied.box_cox_transformation` | boolean | Whether Box-Cox transformation was applied for variance stabilization. |
| `preprocessing_applied.missing_value_imputation` | boolean | Whether missing values were imputed. |
| `preprocessing_applied.decomposition`   | string  | Tipo of decomposition used (additive, multiplicative, or none). |
| `cross_validation_scores`               | object  | Cross-validation performance metrics. |
| `cross_validation_scores.folds`         | int     | Number of cross-validation folds (typically 4). |
| `cross_validation_scores.method`        | string  | Cross-validation method used (rolling_origin). |
| `cross_validation_scores.avg_rmse`      | float   | Average RMSE across all folds. |
| `cross_validation_scores.avg_mae`       | float   | Average MAE across all folds. |
| `models_tested`                         | array   | List of all models tested in the model zoo. |
| `models_tested[].name`                  | string  | Name of the model (ARIMA, Prophet, ETS, TBATS, Croston, Ensemble). |
| `models_tested[].weighted_rmse`         | float   | Weighted RMSE score for the model. |
| `models_tested[].rank`                  | int     | Ranking of the model (1 = best). |
| `performance`                           | object  | Rendimiento metadata. |
| `performance.processing_time_ms`        | int     | Processing time in milliseconds (typical: ~300ms for 90 días). |
| `performance.data_points_used`          | int     | Number of historical data points used in the forecast. |

## Estado HTTP

| Status Código | Descripción |
| ----------- | ----------- |
| `200 OK` | Forecast generated successfully. Respuesta includes dual-view forecasts y model metrics. |
| `400 Bad Request` | Invalid request Parámetros (e.g., missing Requerido fields, invalid data types). |
| `401 Unauthorized` | Missing or invalid Autenticación token. |
| `403 Forbidden` | Valid token but insufficient permissions or invalid tenant credentials. |
| `422 Unprocessable Entity` | Request validation failed (e.g., insufficient data points, invalid date formats, non-numeric revenue values). |
| `429 Too Many Requests` | Límite de tasa excedido. Retry after the specified time. |
| `500 Internal Server Error` | Server-side Error during forecast processing. |
| `503 Service Unavailable` | EchoIntel service temporarily unavailable. |

## Errores

### Error Respuesta Format

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

| Error | Estado HTTP | Cause | Solution |
| ----- | ----------- | ----- | -------- |
| Insufficient data points | `422` | Less than 60 historical data points provided. | Provide at least 60 rows of historical revenue data. |
| All values are zeros | `422` | All revenue values in historical data are 0. | Ensure historical data contains non-zero revenue values. |
| Excessive missing values | `422` | More than 80% of values are missing or null. | Reduce missing values to less than 80% of the dataset. |
| Non-numeric revenue values | `422` | Revenue Campo contains non-numeric data (strings, nulls, etc.). | Ensure all revenue values are valid numbers. |
| Invalid date formats | `422` | Date Campo does not follow ISO 8601 or recognized formats. | Use ISO 8601 format (YYYY-MM-DD) for dates. |
| Invalid granularity | `400` | Granularity value is not one of: daily, weekly, mensual. | Use valid granularity: `daily`, `weekly`, or `monthly`. |
| Invalid confidence level | `400` | Confidence level outside range 0.80-0.99. | Provide confidence level between 0.80 y 0.99. |
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

## Cómo se Calcula

### Model Zoo Approach

The revenue forecasting system employs a **model zoo** methodology, testing six different forecasting algorithms to identify the champion model:

#### 1. ARIMA (Auto-Regressive Integrated Moving Average)
- **Use case:** Time series with trends y autocorrelation
- **Parámetros:** Auto-selected using AIC (Akaike Information Criterion)
- **Strengths:** Excellent for stationary data with clear patterns
- **Weaknesses:** Struggles with multiple seasonality

#### 2. Prophet (Facebook's Time Series Forecasting)
- **Use case:** Time series with strong seasonality y holidías
- **Parámetros:** Automatic seasonality detection, trend changepoints
- **Strengths:** Hyles missing data well, robust to outliers
- **Weaknesses:** Can overfit on short time series

#### 3. ETS (Exponential Smoothing)
- **Use case:** Time series with trend y/or seasonality
- **Parámetros:** Error, Trend, Seasonal components auto-selected
- **Strengths:** Fast, interpretable, good for short-term forecasts
- **Weaknesses:** Less accurate for long-term forecasts

#### 4. TBATS (Trigonometric Seasonality, Box-Cox transformation, ARMA Errores, Trend, Seasonal)
- **Use case:** Complex seasonality patterns (multiple cycles)
- **Parámetros:** Automatic Box-Cox transformation, Fourier terms
- **Strengths:** Hyles multiple seasonal periods
- **Weaknesses:** Computationally intensive

#### 5. Croston's Method
- **Use case:** Intermittent demy forecasting
- **Parámetros:** Smoothing Parámetro alpha
- **Strengths:** Specialized for sparse data
- **Weaknesses:** Not suitable for continuous revenue streams

#### 6. Ensemble (Weighted Average)
- **Use case:** Combining predictions from multiple models
- **Parámetros:** Weights based on individual model performance
- **Strengths:** Reduces variance, improves robustness
- **Weaknesses:** Can be conservative

### Champion Selection Logic

The champion model is selected based on the **lowest weighted RMSE** across both calendar y business day views:

```
weighted_rmse = (calendar_rmse × 0.6) + (business_days_rmse × 0.4)
```

**Rationale:**
- Calendar view (60% weight): Represents the complete picture
- Business días view (40% weight): Focuses on operational días
- The model with the lowest weighted RMSE becomes the champion

### Pre-processing Pipeline

Before model training, the following pre-processing steps are applied:

1. **Outlier Detection via IQR (Interquartile Range)**
   - Identifies values outside Q1 - 1.5×IQR y Q3 + 1.5×IQR
   - Outliers are capped or removed based on severity

2. **Box-Cox Transformation**
   - Stabilizes variance across the time series
   - Automatically determines the optimal lambda Parámetro
   - Applied when heteroscedasticity is detected

3. **Missing Value Imputation**
   - Forward fill for short gaps (1-2 periods)
   - Linear interpolation for medium gaps (3-7 periods)
   - Seasonal average for longer gaps

4. **Trend y Seasonality Decomposition**
   - Separates time series into trend, seasonal, y residual components
   - Uses STL decomposition (Seasonal y Trend decomposition using Loess)
   - Determines additive vs. multiplicative decomposition

### Cross-Validation Strategy

The system uses **rolling origin cross-validation** with walk-forward validation:

- **Folds:** 4
- **Method:** Time series split maintaining temporal order
- **Validation:** Each fold trains on historical data y validates on the next period
- **Metrics:** RMSE y MAE averaged across all folds

### Dual-View Forecasting

The API returns two distinct forecast arrays:

1. **Calendar View (`forecast_calendar`)**
   - Includes all días (weekends, holidías, working días)
   - Useful for financial planning y budgeting
   - Accounts for all temporal periods

2. **Business Days View (`forecast_business_días`)**
   - Excludes weekends y holidías
   - Useful for operational planning
   - Focuses on revenue-generating días

### Optimización de Rendimiento

- **Typical latency:** ~300-400ms for 90 días of historical data
- **Caching:** Model Parámetros cached for similar datasets
- **Parallel processing:** Models trained concurrently in the zoo
- **Early stopping:** Poor-performing models terminated early

## Notas

- **MAPE Interpretation:** Values < 0.10 (10%) are considered excellent, 0.10-0.20 good, 0.20-0.50 acceptable, > 0.50 poor.
- **R² Interpretation:** Values close to 1 indicate better model fit. R² > 0.90 is excellent for revenue forecasting.
- **Confidence Intervals:** Wider intervals indicate higher forecast uncertainty. Consider using lower forecast periods for narrower intervals.
- **Minimum Requisitos de Datos:** At least 60 data points (approximately 2 meses of daily data) Requerido for reliable forecasting.
- **Maximum Forecast Horizon:** 365 días. Longer forecasts may have reduced accuracy.
- **External Factors:** Optional Parámetro that can include seasonality adjustments, promotional impacts, or economic indicators.
- **Granularity Impact:** Daily granularity requires more data points y may have higher variance than monthly granularity.

## Preguntas Frecuentes

### Q: What's the minimum historical data period Requerido for reliable forecasts?
**A:** A minimum of 60 data points is Requerido (approximately 2 meses of daily revenue data). For seasonal products, we recommend 12+ meses of historical data to capture full seasonal cycles. Optimal data spans 24 meses for capturing trend y multiple seasonal patterns. For daily granularity, 180+ días (6 meses) ensures accuracy. If you have less than 60 días, upgrade to weekly or monthly granularity to reduce noise y improve predictions.

### Q: How does the model hyle seasonality (holidías, peak seasons, etc.)?
**A:** The system automatically detects seasonality via STL decomposition y selects appropriate algorithms (Prophet y TBATS excel at seasonal patterns). The Respuesta includes `decomposition` info showing if additive or multiplicative seasonality was detected. For strong seasonal patterns, Prophet typically ranks highest. We detect daily, weekly, monthly, y annual seasonality patterns. If you have custom seasonality (e.g., flash sales), include them in the optional `external_factors` Parámetro for improved accuracy.

### Q: Can I include external factors like holidías, promotions, or economic indicators?
**A:** Sí! Use the optional `external_factors` Parámetro to include impacts like holidías, promotions, marketing campaigns, pricing changes, or economic indicators. Format: `{"holidías": ["2025-12-25", "2025-01-01"], "promotions": [{"date": "2025-11-28", "impact": 0.15}], "price_change": {"date": "2025-01-15", "percent_change": -0.10}}`. External factors improve forecast accuracy by 5-15% when they significantly impact revenue. However, always validate that the impact estimates are reasonable.

### Q: What's the difference between calendar y business días forecasts?
**A:** Calendar view (`forecast_calendar`) includes all días: weekends, holidías, y working días. Use this for financial planning, budgeting, y reporting (total revenue projection). Business días view (`forecast_business_días`) excludes weekends y holidías, showing only operational días. Use this for capacity planning, resource allocation, y operational decisions. Both use the same underlying model but provide different perspectives on the same forecast.

### Q: How do I know which model was selected y why?
**A:** The Respuesta includes `champion_model` Campo showing the selected algorithm (e.g., "Prophet", "ARIMA", "ETS"). The `models_tested` array lists all 6 models with their weighted RMSE scores y rankings. The champion is the one with rank=1 (lowest weighted_rmse). Check the ranking to see if the champion significantly outperformed alternatives. Weighted RMSE combines calendar (60% weight) y business días (40% weight) performance. A significant gap between 1st y 2nd place indicates high confidence in the champion.

### Q: Can I forecast daily revenue or only monthly?
**A:** You can forecast daily, weekly, or monthly via the `granularity` Parámetro. Daily requires the most data (180+ días recommended) y may have higher variance. Weekly is good balance. Monthly is most stable y requires less data. The API hyles all granularities equally. Choose based on your business need y data availability. Daily is ideal for campaign management; monthly for executive reporting y budgeting. The Respuesta structure is identical across granularities.

### Q: How do I interpret the confidence intervals (lower_bound y upper_bound)?
**A:** Confidence intervals represent the model's uncertainty range around the point prediction. Por Defecto 95% confidence means there's a 95% probability the actual value falls within [lower_bound, upper_bound]. Narrower intervals = higher confidence; wider intervals = more uncertainty. As forecast horizons extend (30, 60, 90+ días out), intervals naturally widen. Use lower_bound for conservative planning (worst-case scenario) y upper_bound for optimistic planning. Adjust `confidence_level` (0.80-0.99) to balance tightness vs. reliability.

### Q: What do the RMSE, MAE, MAPE, y R-squared metrics mean?
**A:** RMSE (Root Mean Squared Error) measures average prediction Error in revenue units—lower is better. MAPE (Mean Absolute Percentage Error) shows percentage Error (0-1 scale): < 0.10 is excellent, 0.10-0.20 is good, 0.20-0.50 is acceptable. R-squared shows model fit (0-1): > 0.90 is excellent for revenue forecasting. MAE is the average absolute Error in raw units. All metrics use cross-validation (4 rolling origin folds) to ensure reliability. Compare weighted_rmse across models to see which algorithm is best for your data.

### Q: How do I hyle data quality issues like missing revenue values?
**A:** The system automatically imputes missing values: forward fill for gaps 1-2 periods, linear interpolation for 3-7 periods, y seasonal average for longer gaps. If > 80% of values are missing, the request fails with a clear Error. Outliers are automatically detected via IQR y capped or removed. If you have systematic data quality issues (30%+ missing), clean the data before sending. The Respuesta includes `preprocessing_applied` detailing all transformations so you understy what was corrected.

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

### Step 2: Define Forecast Parámetros

Specify the number of periods to forecast y granularity:

```json
{
  "forecast_periods": 30,
  "granularity": "daily",
  "confidence_level": 0.95
}
```

### Step 3: Make API Request

Send POST request to the Endpoint with Autenticación:

```bash
curl -X POST https://echosistema.online/api/v1/ai/echointel/forecast/revenue \
  -H "Authorization: Bearer {token}" \
  -H "X-Customer-Api-Id: {tenant-uuid}" \
  -H "Content-Type: application/json" \
  -d @request.json
```

### Step 4: Analyze Dual-View Forecasts

Review both calendar y business day forecasts:
- Use `forecast_calendar` for budgeting y financial planning
- Use `forecast_business_días` for operational capacity planning

### Step 5: Evaluate Model Rendimiento

Check the champion model y metrics:
- `champion_model`: Which algorithm performed best
- `weighted_rmse`: Overall prediction accuracy
- `cross_validation_scores`: Validation performance

### Step 6: Understy Preprocessing

Review `preprocessing_applied` to understy data transformations:
- Outlier detection y hyling
- Variance stabilization
- Missing value treatment

### Step 7: Incorporate into Planning

Use forecast data for:
- Revenue projections
- Resource allocation
- Budget planning
- Capacity management

## Relacionado

- [Forecast Units](./ForecastUnits.md) - Unit-based forecasting for inventory planning
- [Forecast Units Asyncio](./ForecastUnitsAsyncio.md) - Asynchronous unit forecasting for high-volume data
- [Forecast Cost](./ForecastCost.md) - Cost forecasting using time series analysis
- [Forecast Cost Improved](./ForecastCostImproved.md) - Enhanced cost forecasting with advanced algorithms
- [Customer CLV Forecast](../CustomerIntelligence/CustomerClvForecast.md) - Customer Lifetime Value prediction

## Referencias

* Controlador: `src/Domain/ArtificialIntelligence/Http/Controllers/EchoIntelProxyController.php:158`
* Endpoint Method: `forecastRevenue(Request $request): JsonResponse`
* Route: `POST /api/v1/ai/echointel/forecast/revenue`
