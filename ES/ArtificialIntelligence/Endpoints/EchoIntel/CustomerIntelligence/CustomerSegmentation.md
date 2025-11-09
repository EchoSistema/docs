# Inteligencia Artificial – Segmentación de Clientes

## Endpoint

```
POST /api/v1/ai/echointel/customer-intelligence/segmentation
```

Realiza análisis de segmentación de clientes utilizando técnicas de machine learning para identificar grupos de clientes con características similares.

## Autenticación

Requerido – Bearer {token} con middleware `auth:sanctum`

## Encabezados

| Encabezado          | Tipo   | Requerido | Descripción |
| ------------------ | ------ | ----------- | --------- |
| Authorization      | string | Sí         | `Bearer {token}`. |
| X-Customer-Api-Id  | string | Condicional | UUID del tenant (v4). Requerido si en el está configurado en el servidor. |
| X-Secret           | string | Condicional | Secret de 64 caracteres. Requerido si en el está configurado en el servidor. |
| Accept-Language    | string | No         | Idioma de la respuesta (`en`, `es`, `pt`). Predeterminado: `en`. |
| Content-Type       | string | Sí         | `application/json`. |

## Parámetros

### Parámetros del cuerpo

Los parámetros varían según los requisitos del algoritmo de segmentación. Consulte la documentación de la API EchoIntel para detalles específicos.

| Parámetro    | Tipo   | Requerido | Descripción |
| ------------ | ------ | ----------- | --------- |
| data         | array  | Sí         | Array de datos de los clientes para segmentación. |
| algorithm    | string | No         | Algoritmo a utilizar (ex.: `kmeans`, `hierarchical`). |
| n_clusters   | int    | No         | Número de clusters deseados. |
| features     | array  | No         | Features específicas para análisis. |

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
    "data": [
      {"customer_id": "123", "total_purchases": 1500, "frequency": 12},
      {"customer_id": "456", "total_purchases": 3000, "frequency": 24}
    ],
    "algorithm": "kmeans",
    "n_clusters": 3
  }' \
  "https://echosistema.online/api/v1/ai/echointel/customer-intelligence/segmentation"
```

### Ejemplo de solicitud (JavaScript)

```javascript
const response = await fetch('https://echosistema.online/api/v1/ai/echointel/customer-intelligence/segmentation', {
  method: 'POST',
  headers: {
    'Authorization': 'Bearer <token>',
    'X-Customer-Api-Id': '<tenant-uuid>',
    'X-Secret': '<secret>',
    'Accept-Language': 'pt',
    'Content-Type': 'application/json',
  },
  body: JSON.stringify({
    data: [
      {customer_id: '123', total_purchases: 1500, frequency: 12},
      {customer_id: '456', total_purchases: 3000, frequency: 24}
    ],
    algorithm: 'kmeans',
    n_clusters: 3
  })
});

const result = await response.json();
console.log(result);
```

### Ejemplo de solicitud (PHP)

```php
<?php

use Illuminate\Support\Facades\Http;

$response = Http::withHeaders([
    'Authorization' => 'Bearer ' . $token,
    'X-Customer-Api-Id' => $tenantUuid,
    'X-Secret' => $secret,
    'Accept-Language' => 'pt',
])->post('https://echosistema.online/api/v1/ai/echointel/customer-intelligence/segmentation', [
    'data' => [
        ['customer_id' => '123', 'total_purchases' => 1500, 'frequency' => 12],
        ['customer_id' => '456', 'total_purchases' => 3000, 'frequency' => 24],
    ],
    'algorithm' => 'kmeans',
    'n_clusters' => 3,
]);

$result = $response->json();
```

## Respuesta

### Éxito `200 OK`

```json
{
  "segments": [
    {
      "segment_id": 0,
      "size": 150,
      "characteristics": {
        "avg_purchases": 1200,
        "avg_frequency": 10
      },
      "customers": ["123", "789"]
    },
    {
      "segment_id": 1,
      "size": 200,
      "characteristics": {
        "avg_purchases": 3500,
        "avg_frequency": 25
      },
      "customers": ["456"]
    }
  ],
  "metrics": {
    "silhouette_score": 0.72,
    "davies_bouldin_score": 0.45
  }
}
```

### Error `400 Bad Request`

```json
{
  "error": "Invalid parameters",
  "message": "The data field is required."
}
```

### Error `401 Unauthorized`

```json
{
  "error": "Unauthorized",
  "message": "Invalid authentication credentials"
}
```

### Error `500 Internal Server Error`

```json
{
  "error": "Failed to communicate with AI service",
  "message": "Internal server error"
}
```

## Estructura JSON

| Campo                              | Tipo    | Descripción |
| ---------------------------------- | ------- | --------- |
| `segments`                         | array   | Array de segmentos identificados. |
| `segments[].segment_id`            | int     | Identificador del segmento. |
| `segments[].size`                  | int     | Número de clientes en el segmento. |
| `segments[].characteristics`       | object  | Características promedio del segmento. |
| `segments[].customers`             | array   | IDs de los clientes en el segmento. |
| `metrics`                          | object  | Métricas de calidad de la segmentación. |
| `metrics.silhouette_score`         | float   | Score de silhouette (0-1, mayor é melhor). |
| `metrics.davies_bouldin_score`     | float   | Score Davies-Bouldin (menor es mejor). |

## Notas

* El secret debe rotarse cada 90 días según la política de seguridad.
* El tiempo de procesamiento puede variar según el volumen de datos (timeout: 300 segundos).
* Los headers `X-Customer-Api-Id` e `X-Secret` pueden configurarse en el servidor mediante `.env`.
* La respuesta puede variar según la API EchoIntel. Consulte la documentación oficial para detalles específicos.

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

* Controller: `src/Domain/ArtificialIntelligence/Http/Controllers/EchoIntelProxyController.php:130`
