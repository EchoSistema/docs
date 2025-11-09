# Inteligencia Artificial – Segmentación de Compras

## Endpoint

```
POST /api/v1/ai/echointel/customer-intelligence/purchasing-segmentation
```

Realiza segmentación de clientes con base em padrões de compra, identificando grupos con comportamentos similares.

## Autenticación

Requerido – Bearer {token} con middleware `auth:sanctum`

## Encabezados

| Encabezado          | Tipo   | Requerido | Descripción |
| ------------------ | ------ | ----------- | --------- |
| Authorization      | string | Sí         | `Bearer {token}`. |
| X-Customer-Api-Id  | string | Condicional | UUID del tenant (v4). Requerido si en el está configurado en el servidor. |
| X-Secret           | string | Condicional | Secret de 64 caracteres. Requerido si en el está configurado en el servidor. |
| Accept-Language    | string | No         | Idioma (`en`, `es`, `pt`). Predeterminado: `en`. |
| Content-Type       | string | Sí         | `application/json`. |

## Parámetros

### Parámetros del cuerpo

| Parámetro          | Tipo   | Requerido | Descripción |
| ------------------ | ------ | ----------- | --------- |
| customer_data      | array  | Sí         | Datos de los clientes para segmentación. |
| segmentation_type  | string | No         | Tipo de segmentación (`behavioral`, `value`, `frequency`). |
| time_period        | object | No         | Período temporal para análisis. |

> **Nota:** Os parâmetros aceitam tanto `snake_case` quanto `camelCase` (ex.: `customer_data` ou `customerData`).

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
    "customer_data": [
      {"customer_id": "C001", "total_purchases": 1500, "avg_order_value": 150},
      {"customer_id": "C002", "total_purchases": 3000, "avg_order_value": 300}
    ],
    "segmentation_type": "behavioral"
  }' \
  "https://echosistema.online/api/v1/ai/echointel/customer-intelligence/purchasing-segmentation"
```

## Respuesta

### Éxito `200 OK`

```json
{
  "segments": [
    {
      "segment_id": "high_value",
      "customer_count": 150,
      "avg_purchase_value": 2500,
      "customers": ["C002"]
    },
    {
      "segment_id": "regular",
      "customer_count": 300,
      "avg_purchase_value": 800,
      "customers": ["C001"]
    }
  ]
}
```

## Notas

* Segmentación baseada em algoritmos de machine learning.
* Suporta múltiplos critérios de segmentación.
* Parâmetros aceitam `snake_case` ou `camelCase`.

## Cómo se Calcula

El sistema utiliza clustering algorithms (K-means, hierarchical clustering) para group similar customers or items into segments.

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

* Controller: `src/Domain/ArtificialIntelligence/Http/Controllers/EchoIntelProxyController.php:105`
