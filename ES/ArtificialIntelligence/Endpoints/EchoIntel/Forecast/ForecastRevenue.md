# Inteligencia Artificial – Previsión de Ingresos

## Endpoint

```
POST /api/v1/ai/echointel/forecast/revenue
```

Realiza previsión de receita futura utilizando series temporales y modelos de machine learning.

## Autenticación

Requerido – Bearer {token} con middleware `auth:sanctum`

## Encabezados

| Encabezado          | Tipo   | Requerido | Descripción |
| ------------------ | ------ | ----------- | --------- |
| Authorization      | string | Sí         | `Bearer {token}`. |
| X-Customer-Api-Id  | string | Condicional | UUID del tenant (v4). |
| X-Secret           | string | Condicional | Secret de 64 caracteres. |
| Accept-Language    | string | No         | Idioma (`en`, `es`, `pt`). |
| Content-Type       | string | Sí         | `application/json`. |

## Parámetros

### Parámetros del cuerpo

| Parámetro          | Tipo   | Requerido | Descripción |
| ------------------ | ------ | ----------- | --------- |
| historical_revenue | array  | Sí         | Datos históricos de receita. |
| forecast_periods   | int    | Sí         | Número de períodos futuros a prever. |
| granularity        | string | No         | Granularidade temporal (`daily`, `weekly`, `monthly`). Predeterminado: `monthly`. |
| confidence_level   | float  | No         | Nivel de confianza para intervalos (0-1). Predeterminado: `0.95`. |
| external_factors   | object | No         | Factores externos (sazonalidade, promoções, etc.). |

## Ejemplos

### Ejemplo de solicitud (curl)

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

```json
{
  "forecast": [
    {
      "period": "2025-01",
      "predicted_revenue": 205000,
      "lower_bound": 192000,
      "upper_bound": 218000,
      "confidence": 0.95
    },
    {
      "period": "2025-02",
      "predicted_revenue": 212000,
      "lower_bound": 197500,
      "upper_bound": 226500,
      "confidence": 0.95
    },
    {
      "period": "2025-03",
      "predicted_revenue": 218000,
      "lower_bound": 202000,
      "upper_bound": 234000,
      "confidence": 0.95
    }
  ],
  "total_forecast": {
    "sum": 1272000,
    "avg": 212000,
    "growth_rate": 0.085
  },
  "model_metrics": {
    "mae": 4250.5,
    "rmse": 5890.3,
    "mape": 0.029,
    "r_squared": 0.94
  },
  "model_type": "ARIMA_SARIMA"
}
```

## Estructura JSON

| Campo                          | Tipo    | Descripción |
| ------------------------------ | ------- | --------- |
| `forecast`                     | array   | Previsiones por período. |
| `forecast[].period`            | string  | Período de la previsión. |
| `forecast[].predicted_revenue` | float   | Ingresos previstos. |
| `forecast[].lower_bound`       | float   | Límite inferior del intervalo de confianza. |
| `forecast[].upper_bound`       | float   | Límite superior del intervalo de confianza. |
| `forecast[].confidence`        | float   | Nivel de confianza. |
| `total_forecast`               | object  | Métricas agregadas. |
| `total_forecast.sum`           | float   | Suma total prevista. |
| `total_forecast.avg`           | float   | Promedio previsto. |
| `total_forecast.growth_rate`   | float   | Tasa de crecimiento estimada. |
| `model_metrics`                | object  | Métricas de rendimiento. |
| `model_metrics.mae`            | float   | Mean Absolute Error. |
| `model_metrics.rmse`           | float   | Root Mean Squared Error. |
| `model_metrics.mape`           | float   | Mean Absolute Percentage Error. |
| `model_metrics.r_squared`      | float   | R² (0-1). |
| `model_type`                   | string  | Tipo de modelo utilizado. |

## Notas

* MAPE bajo indica mejor precisión (< 0.10 é considerado excelente).
* R² cercano a 1 indica mejor ajuste del modelo.
* Los intervalos de confianza indican la incertidumbre de la previsión.

## Referencias

* Controller: `src/Domain/ArtificialIntelligence/Http/Controllers/EchoIntelProxyController.php:224`
