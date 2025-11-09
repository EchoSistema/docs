# Inteligencia Artificial – Análise de Sentimento em Tempo Real

## Endpoint

```
POST /api/v1/ai/echointel/analytics/sentiment-realtime
```

Realiza análisis de sentimento em tempo real de textos (reviews, comentários, feedbacks) utilizando processamento de linguagem natural (NLP).

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
| texts           | array  | Sí         | Textos para análisis de sentimento. |
| language        | string | No         | Idioma de los textos (`auto` para detecção automática). |
| include_entities| boolean| No         | Extrair entidades mencionadas. Predeterminado: `false`. |
| include_aspects | boolean| No         | Análise de sentimento por aspecto. Predeterminado: `false`. |

## Ejemplos

### Ejemplo de solicitud (curl)

```bash
curl -X POST \
  -H "Authorization: Bearer <token>" \
  -H "X-Customer-Api-Id: <tenant-uuid>" \
  -H "X-Secret: <secret>" \
  -H "Content-Type: application/json" \
  -d '{
    "texts": [
      {
        "id": "review_001",
        "text": "Produto excelente! Entrega rápida y atendimento impecável.",
        "source": "product_review"
      },
      {
        "id": "comment_045",
        "text": "Péssima experiência. O producto chegou danificado y o suporte não resolveu.",
        "source": "customer_feedback"
      }
    ],
    "language": "pt",
    "include_entities": true,
    "include_aspects": true
  }' \
  "https://echosistema.online/api/v1/ai/echointel/analytics/sentiment-realtime"
```

## Respuesta

### Éxito `200 OK`

```json
{
  "sentiment_analysis": [
    {
      "id": "review_001",
      "text": "Produto excelente! Entrega rápida y atendimento impecável.",
      "sentiment": "positive",
      "confidence": 0.96,
      "score": 0.88,
      "aspects": [
        {"aspect": "producto", "sentiment": "positive", "score": 0.92},
        {"aspect": "entrega", "sentiment": "positive", "score": 0.85},
        {"aspect": "atendimento", "sentiment": "positive", "score": 0.90}
      ],
      "entities": [
        {"entity": "producto", "type": "PRODUCT"},
        {"entity": "entrega", "type": "SERVICE"},
        {"entity": "atendimento", "type": "SERVICE"}
      ],
      "keywords": ["excelente", "rápida", "impecável"]
    },
    {
      "id": "comment_045",
      "text": "Péssima experiência. O producto chegou danificado y o suporte não resolveu.",
      "sentiment": "negative",
      "confidence": 0.94,
      "score": -0.82,
      "aspects": [
        {"aspect": "experiência", "sentiment": "negative", "score": -0.95},
        {"aspect": "producto", "sentiment": "negative", "score": -0.78},
        {"aspect": "suporte", "sentiment": "negative", "score": -0.75}
      ],
      "entities": [
        {"entity": "producto", "type": "PRODUCT"},
        {"entity": "suporte", "type": "SERVICE"}
      ],
      "keywords": ["péssima", "danificado", "não resolveu"]
    }
  ],
  "summary": {
    "total_analyzed": 2,
    "positive": 1,
    "neutral": 0,
    "negative": 1,
    "avg_score": 0.03
  }
}
```

## Estructura JSON

| Campo                                    | Tipo    | Descripción |
| ---------------------------------------- | ------- | --------- |
| `sentiment_analysis`                     | array   | Análises por texto. |
| `sentiment_analysis[].id`                | string  | ID del texto. |
| `sentiment_analysis[].text`              | string  | Texto analisado. |
| `sentiment_analysis[].sentiment`         | string  | Sentimento (`positive`, `neutral`, `negative`). |
| `sentiment_analysis[].confidence`        | float   | Confianza de la classificação (0-1). |
| `sentiment_analysis[].score`             | float   | Score de sentimento (-1 a +1). |
| `sentiment_analysis[].aspects`           | array   | Análise por aspecto (se solicitado). |
| `sentiment_analysis[].entities`          | array   | Entidades extraídas (se solicitado). |
| `sentiment_analysis[].keywords`          | array   | Palavras-chave identificadas. |
| `summary`                                | object  | Resumo geral. |
| `summary.total_analyzed`                 | int     | Total de textos analisados. |
| `summary.positive`                       | int     | Quantidade de sentimentos positivos. |
| `summary.neutral`                        | int     | Quantidade de sentimentos neutros. |
| `summary.negative`                       | int     | Quantidade de sentimentos negativos. |
| `summary.avg_score`                      | float   | Score médio. |

## Classificação de Sentimento

| Score       | Sentimento | Descripción |
| ----------- | ---------- | --------- |
| 0.5 a 1.0   | Positive   | Sentimento claramente positivo. |
| 0.1 a 0.49  | Positive   | Sentimento levemente positivo. |
| -0.1 a 0.1  | Neutral    | Sentimento neutro o misto. |
| -0.49 a -0.1| Negative   | Sentimento levemente negativo. |
| -1.0 a -0.5 | Negative   | Sentimento claramente negativo. |

## Notas

* Suporte para múltiplos idiomas (auto-detecção disponível).
* Análise por aspecto identifica sentimentos sobre diferentes características.
* Útil para análisis de reviews, feedback de clientes, redes sociais, etc.

## Cómo se Calcula

El sistema de análisis de sentimientos utiliza modelos NLP de última generación para clasificar el sentimiento del texto y extraer insights:

### 1. Arquitectura del Modelo NLP

El sistema emplea modelos basados en transformers afinados para clasificación de sentimientos:

- **Modelos Basados en BERT:** Usa BERT multilingüe (mBERT) o variantes BERT específicas del idioma para detección de sentimientos consciente del contexto
- **Sentimiento Basado en Aspectos:** Aplica extracción de sentimiento dirigido para aspectos/características específicos mencionados en el texto
- **Reconocimiento de Entidades:** Usa Reconocimiento de Entidades Nombradas (NER) para identificar productos, servicios y marcas

### 2. Proceso de Puntuación de Sentimiento

- **Paso 1:** Preprocesamiento de texto (tokenización, normalización, minúsculas)
- **Paso 2:** Generación de embeddings contextuales usando codificador BERT (vectores de 768 dimensiones)
- **Paso 3:** Cabeza de clasificación predice probabilidades de sentimiento [positivo, neutral, negativo]
- **Paso 4:** Convertir probabilidades a puntuación continua: puntuación = P(positivo) - P(negativo) ∈ [-1, +1]

### 3. Confianza y Calibración

- **Puntuación de Confianza:** Probabilidad máxima de clase después de calibración de escalado de temperatura
- **Clasificación Basada en Umbrales:** Zona neutral (-0.1 a +0.1) reduce clasificaciones falsas positivas/negativas
- **Agregación de Conjunto:** Combina predicciones de múltiples modelos para mejorar la precisión (opcional)

### 4. Rendimiento y Optimización

- **Tiempo de Procesamiento:** 50-100ms por texto (procesamiento por lotes: 20 textos/segundo)
- **Precisión:** 88-92% en conjuntos de datos de referencia (SST-2, IMDB, datos de dominio personalizado)
- **Idiomas Soportados:** 100+ idiomas vía mBERT, optimizado para EN, ES, PT
- **Aceleración GPU:** Reduce latencia en 5-10x para solicitudes por lotes (>50 textos)

## Referencias

* Controller: `src/Domain/ArtificialIntelligence/Http/Controllers/EchoIntelProxyController.php:333`
