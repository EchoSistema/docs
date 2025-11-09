# Inteligencia Artificial – Recomendación de Itens para Usuário

## Endpoint

```
POST /api/v1/ai/echointel/recommendations/user-items
```

Gera recomendações personalizadas de productos/itens para usuários específicos baseado em collaborative filtering y content-based filtering.

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

| Parámetro     | Tipo   | Requerido | Descripción |
| ------------- | ------ | ----------- | --------- |
| user_id       | string | Sí         | ID del usuário. |
| n_recommendations | int | No       | Número de recomendações. Predeterminado: `10`. |
| exclude_purchased | boolean | No   | Excluir itens já comprados. Predeterminado: `true`. |
| filters       | object | No         | Filtros adicionais (categoria, preço, etc.). |

## Ejemplos

### Ejemplo de solicitud (curl)

```bash
curl -X POST \
  -H "Authorization: Bearer <token>" \
  -H "X-Customer-Api-Id: <tenant-uuid>" \
  -H "X-Secret: <secret>" \
  -H "Content-Type: application/json" \
  -d '{
    "user_id": "U001",
    "n_recommendations": 5,
    "exclude_purchased": true,
    "filters": {
      "category": "electronics",
      "max_price": 1000
    }
  }' \
  "https://echosistema.online/api/v1/ai/echointel/recommendations/user-items"
```

## Respuesta

### Éxito `200 OK`

```json
{
  "user_id": "U001",
  "recommendations": [
    {
      "item_id": "ITEM-789",
      "score": 0.94,
      "rank": 1,
      "reason": "Based on your recent purchases and similar users",
      "item_details": {
        "name": "Wireless Headphones Pro",
        "category": "electronics",
        "price": 299.99
      }
    },
    {
      "item_id": "ITEM-456",
      "score": 0.88,
      "rank": 2,
      "reason": "Frequently bought together with your previous items",
      "item_details": {
        "name": "Smartphone Case Premium",
        "category": "accessories",
        "price": 49.99
      }
    }
  ],
  "algorithm_used": "hybrid_collaborative_content",
  "confidence": 0.91
}
```

## Estructura JSON

| Campo                              | Tipo    | Descripción |
| ---------------------------------- | ------- | --------- |
| `user_id`                          | string  | ID del usuário. |
| `recommendations`                  | array   | Lista de recomendações. |
| `recommendations[].item_id`        | string  | ID del item recomendado. |
| `recommendations[].score`          | float   | Score de relevância (0-1). |
| `recommendations[].rank`           | int     | Ranking de la recomendação. |
| `recommendations[].reason`         | string  | Razão de la recomendação. |
| `recommendations[].item_details`   | object  | Detalhes del item. |
| `algorithm_used`                   | string  | Algoritmo utilizado. |
| `confidence`                       | float   | Confianza geral (0-1). |

## Notas

* Recomendações são ordenadas por score descendente.
* Algoritmos disponíveis: `collaborative`, `content_based`, `hybrid`.
* Recomendações são atualizadas em tempo real conforme novas interações.

## Cómo se Calcula

El sistema utiliza collaborative filtering and content-based algorithms para generate personalized recommendations.

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

* Controller: `src/Domain/ArtificialIntelligence/Http/Controllers/EchoIntelProxyController.php:192`
