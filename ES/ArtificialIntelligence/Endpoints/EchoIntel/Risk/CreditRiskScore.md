# Inteligencia Artificial – Score de Riesgo de Crédito

## Endpoint

```
POST /api/v1/ai/echointel/risk/credit-score
```

Calcula score de risco de crédito para clientes utilizando modelos de machine learning que analizan múltiples variables financeiras y comportamentais.

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
| applicants         | array  | Sí         | Datos de los solicitantes de crédito. |
| credit_amount      | float  | Sí         | Valor del crédito solicitado. |
| include_explanation| boolean| No         | Incluir explicación detallada. Predeterminado: `false`. |

## Ejemplos

### Ejemplo de solicitud (curl)

```bash
curl -X POST \
  -H "Authorization: Bearer <token>" \
  -H "X-Customer-Api-Id: <tenant-uuid>" \
  -H "X-Secret: <secret>" \
  -H "Content-Type: application/json" \
  -d '{
    "applicants": [
      {
        "applicant_id": "A001",
        "age": 35,
        "annual_income": 75000,
        "employment_length": 8,
        "debt_to_income": 0.28,
        "credit_history_length": 12,
        "previous_defaults": 0,
        "open_credit_lines": 3
      }
    ],
    "credit_amount": 25000,
    "include_explanation": true
  }' \
  "https://echosistema.online/api/v1/ai/echointel/risk/credit-score"
```

## Respuesta

### Éxito `200 OK`

```json
{
  "credit_scores": [
    {
      "applicant_id": "A001",
      "credit_score": 742,
      "risk_category": "low",
      "approval_recommendation": "approve",
      "default_probability": 0.08,
      "suggested_terms": {
        "max_amount": 30000,
        "interest_rate": 0.065,
        "term_months": 60
      },
      "risk_factors": [
        {
          "factor": "stable employment",
          "impact": "positive",
          "weight": 0.25
        },
        {
          "factor": "low debt-to-income ratio",
          "impact": "positive",
          "weight": 0.22
        },
        {
          "factor": "no previous defaults",
          "impact": "positive",
          "weight": 0.20
        }
      ]
    }
  ],
  "model_version": "v2.5.3",
  "evaluation_date": "2025-01-07T15:30:00Z"
}
```

## Estructura JSON

| Campo                                      | Tipo    | Descripción |
| ------------------------------------------ | ------- | --------- |
| `credit_scores`                            | array   | Scores por solicitante. |
| `credit_scores[].applicant_id`             | string  | ID del solicitante. |
| `credit_scores[].credit_score`             | int     | Score de crédito (300-850). |
| `credit_scores[].risk_category`            | string  | Categoría de risco (`low`, `medium`, `high`). |
| `credit_scores[].approval_recommendation`  | string  | Recomendación (`approve`, `review`, `reject`). |
| `credit_scores[].default_probability`      | float   | Probabilidad de default (0-1). |
| `credit_scores[].suggested_terms`          | object  | Términos sugeridos. |
| `credit_scores[].suggested_terms.max_amount` | float | Valor máximo recomendado. |
| `credit_scores[].suggested_terms.interest_rate` | float | Tasa de interés sugerida. |
| `credit_scores[].suggested_terms.term_months` | int  | Plazo en meses. |
| `credit_scores[].risk_factors`             | array   | Factores de riesgo. |

## Categorías de Riesgo

| Score      | Categoría | Descripción |
| ---------- | --------- | --------- |
| 750-850    | Low       | Riesgo muy bajo, excelente historial. |
| 650-749    | Medium-Low| Riesgo bajo, buen historial. |
| 550-649    | Medium    | Riesgo moderado, requer análisis. |
| 450-549    | Medium-High| Riesgo elevado, análisis criteriosa necessária. |
| 300-449    | High      | Riesgo muy alto, aprobación en el recomendada. |

## Notas

* Scores más altos indican menor risco de crédito.
* Factores considerados: historial crediticio, ingresos, empleo, deudas, etc.
* Para explicaciones detalladas, use `include_explanation: true`.

## Cómo se Calcula

El sistema utiliza risk scoring and classification models para assess credit risk and default probability.

### 1. Algoritmo Principal

- Utiliza técnicas de aprendizaje automático estándar de la industria
- Entrenado en patrones de datos históricos
- Optimizado para precisión y rendimiento

### 2. Pasos de Procesamiento

- **Paso 1:** Preprocesamiento de datos y extracción de características
- **Paso 2:** Entrenamiento o inferencia del modelo
- **Paso 3:** Generación y validación de resultados
- **Paso 4:** Formateo y entrega de salida

### 3. Rendimiento

- **Tiempo de Procesamiento:** Optimizado para respuesta sub-segundo (típico: 200-500ms)
- **Escalabilidad:** Maneja grandes conjuntos de datos eficientemente
- **Precisión:** Validado contra conjuntos de datos de referencia

## Referencias

* Controller: `src/Domain/ArtificialIntelligence/Http/Controllers/EchoIntelProxyController.php:288`
