# Inteligência Artificial – Previsão de Receita

## Endpoint

```
POST /api/v1/ai/echointel/forecast/revenue
```

Realiza previsão de receita futura utilizando séries temporais e modelos de machine learning.

## Autenticação

Obrigatório – Bearer {token} com middleware `auth:sanctum`

## Cabeçalhos

| Cabeçalho          | Tipo   | Obrigatório | Descrição |
| ------------------ | ------ | ----------- | --------- |
| Authorization      | string | Sim         | `Bearer {token}`. |
| X-Customer-Api-Id  | string | Condicional | UUID do tenant (v4). |
| X-Secret           | string | Condicional | Secret de 64 caracteres. |
| Accept-Language    | string | Não         | Idioma (`en`, `es`, `pt`). |
| Content-Type       | string | Sim         | `application/json`. |

## Parâmetros

### Parâmetros do corpo

| Parâmetro          | Tipo   | Obrigatório | Descrição |
| ------------------ | ------ | ----------- | --------- |
| historical_revenue | array  | Sim         | Dados históricos de receita. |
| forecast_periods   | int    | Sim         | Número de períodos futuros a prever. |
| granularity        | string | Não         | Granularidade temporal (`daily`, `weekly`, `monthly`). Padrão: `monthly`. |
| confidence_level   | float  | Não         | Nível de confiança para intervalos (0-1). Padrão: `0.95`. |
| external_factors   | object | Não         | Fatores externos (sazonalidade, promoções, etc.). |

## Exemplos

### Exemplo de requisição (curl)

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
  "https://your-domain.com/api/v1/ai/echointel/forecast/revenue"
```

## Resposta

### Sucesso `200 OK`

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

## Estrutura JSON

| Campo                          | Tipo    | Descrição |
| ------------------------------ | ------- | --------- |
| `forecast`                     | array   | Previsões por período. |
| `forecast[].period`            | string  | Período da previsão. |
| `forecast[].predicted_revenue` | float   | Receita prevista. |
| `forecast[].lower_bound`       | float   | Limite inferior do intervalo de confiança. |
| `forecast[].upper_bound`       | float   | Limite superior do intervalo de confiança. |
| `forecast[].confidence`        | float   | Nível de confiança. |
| `total_forecast`               | object  | Métricas agregadas. |
| `total_forecast.sum`           | float   | Soma total prevista. |
| `total_forecast.avg`           | float   | Média prevista. |
| `total_forecast.growth_rate`   | float   | Taxa de crescimento estimada. |
| `model_metrics`                | object  | Métricas de performance. |
| `model_metrics.mae`            | float   | Mean Absolute Error. |
| `model_metrics.rmse`           | float   | Root Mean Squared Error. |
| `model_metrics.mape`           | float   | Mean Absolute Percentage Error. |
| `model_metrics.r_squared`      | float   | R² (0-1). |
| `model_type`                   | string  | Tipo de modelo utilizado. |

## Notas

* MAPE baixo indica melhor precisão (< 0.10 é considerado excelente).
* R² próximo de 1 indica melhor ajuste do modelo.
* Intervalos de confiança indicam a incerteza da previsão.

## Referências

* [Documentação EchoIntel](https://github.com/EchoSistema/abintel-documentation)
* Controller: `src/Domain/ArtificialIntelligence/Http/Controllers/EchoIntelProxyController.php:224`
