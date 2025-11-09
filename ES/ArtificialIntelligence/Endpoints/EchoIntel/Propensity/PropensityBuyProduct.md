# Inteligencia Artificial – Propensión de Compra de Producto

## Endpoint

```
POST /api/v1/ai/echointel/propensity/buy-product
```

Calcula a propensión de clientes comprar productos específicos utilizando modelos predictivos.

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

| Parámetro       | Tipo   | Requerido | Descripción |
| --------------- | ------ | ----------- | --------- |
| customers       | array  | Sí         | Lista de clientes para análisis. |
| product_id      | string | Sí         | ID del producto objetivo. |
| historical_data | array  | No         | Datos históricos para mejorar precisión. |
| top_n           | int    | No         | Número de clientes con mayor propensión a retornar. Predeterminado: `100`. |

## Ejemplos

### Ejemplo de solicitud (curl)

```bash
curl -X POST \
  -H "Authorization: Bearer <token>" \
  -H "X-Customer-Api-Id: <tenant-uuid>" \
  -H "X-Secret: <secret>" \
  -H "Accept-Language: pt" \
  -H "Content-Type: application/json" \
  -d '{
    "customers": [
      {"customer_id": "C001", "age": 35, "income": 75000, "previous_purchases": 12},
      {"customer_id": "C002", "age": 28, "income": 55000, "previous_purchases": 5}
    ],
    "product_id": "PROD-123",
    "top_n": 50
  }' \
  "https://echosistema.online/api/v1/ai/echointel/propensity/buy-product"
```

## Respuesta

### Éxito `200 OK`

```json
{
  "propensity_scores": [
    {
      "customer_id": "C001",
      "propensity_score": 0.87,
      "propensity_level": "high",
      "confidence": 0.92,
      "key_factors": [
        {"factor": "high previous purchases", "weight": 0.35},
        {"factor": "similar product history", "weight": 0.28},
        {"factor": "demographic match", "weight": 0.24}
      ],
      "recommended_actions": [
        "Send personalized email with product details",
        "Offer limited-time discount",
        "Highlight product benefits"
      ]
    },
    {
      "customer_id": "C002",
      "propensity_score": 0.42,
      "propensity_level": "medium",
      "confidence": 0.78,
      "key_factors": [
        {"factor": "age group match", "weight": 0.32},
        {"factor": "income bracket", "weight": 0.25}
      ],
      "recommended_actions": [
        "Include in general marketing campaign",
        "Provide educational content about product"
      ]
    }
  ],
  "product_info": {
    "product_id": "PROD-123",
    "avg_propensity": 0.645,
    "total_high_propensity": 125,
    "total_medium_propensity": 342,
    "total_low_propensity": 533
  }
}
```

## Estructura JSON

| Campo                                       | Tipo    | Descripción |
| ------------------------------------------- | ------- | --------- |
| `propensity_scores`                         | array   | Scores de propensión por cliente. |
| `propensity_scores[].customer_id`           | string  | ID del cliente. |
| `propensity_scores[].propensity_score`      | float   | Score de propensión (0-1). |
| `propensity_scores[].propensity_level`      | string  | Nivel (`low`, `medium`, `high`). |
| `propensity_scores[].confidence`            | float   | Confianza de la predicción (0-1). |
| `propensity_scores[].key_factors`           | array   | Factores clave de la propensión. |
| `propensity_scores[].recommended_actions`   | array   | Acciones recomendadas. |
| `product_info`                              | object  | Información agregada del producto. |
| `product_info.avg_propensity`               | float   | Propensión promedio. |
| `product_info.total_high_propensity`        | int     | Total de clientes con alta propensión. |

## Notas

* Níveis de propensión: `low` (< 0.3), `medium` (0.3-0.7), `high` (> 0.7).
* Resultados ordenados por `propensity_score` descendente.
* Recomendações são personalizadas por nível de propensión.

## Cómo se Calcula

El sistema utiliza classification models (logistic regression, random forest) para predict likelihood of specific customer actions.

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

* Controller: `src/Domain/ArtificialIntelligence/Http/Controllers/EchoIntelProxyController.php:177`
