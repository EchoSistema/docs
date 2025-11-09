# Inteligencia Artificial – Relatório de Sentimento

## Endpoint

```
POST /api/v1/ai/echointel/analytics/sentiment-report
```

Relatório de Sentimento consolidando múltiplas fontes.

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

> **Nota:** Os parâmetros aceitam tanto `snake_case` quanto `camelCase`.


## Cómo se Calcula

El informe de sentimientos agrega y analiza datos de sentimientos de múltiples fuentes para proporcionar insights completos:

### 1. Agregación de Datos

El sistema consolida puntuaciones de sentimientos de varias fuentes de datos:

- **Colección Multi-Fuente:** Agrega sentimientos de reseñas, redes sociales, retroalimentación de clientes, tickets de soporte, encuestas
- **Agrupación Temporal:** Agrupa sentimientos por períodos de tiempo (horario, diario, semanal, mensual) para análisis de tendencias
- **Ponderación de Fuentes:** Aplica pesos configurables a diferentes fuentes basados en confiabilidad e importancia empresarial

### 2. Análisis Estadístico

- **Paso 1:** Calcular estadísticas descriptivas (media, mediana, desviación estándar) para puntuaciones de sentimientos
- **Paso 2:** Calcular distribución de sentimientos (% positivo, % neutral, % negativo) a través de períodos de tiempo
- **Paso 3:** Identificar tendencias estadísticamente significativas usando prueba de tendencia Mann-Kendall
- **Paso 4:** Detectar anomalías de sentimiento usando detección de valores atípicos basada en puntuación Z e IQR

### 3. Generación de Insights

- **Detección de Tendencias:** Identifica patrones de sentimientos en mejora, declive o estables a lo largo del tiempo
- **Extracción de Temas:** Usa LDA (Latent Dirichlet Allocation) para descubrir temas principales en retroalimentación positiva/negativa
- **Análisis Comparativo:** Compara sentimientos entre productos, regiones, segmentos de clientes o períodos de tiempo
- **Disparadores de Alerta:** Señala caídas o picos repentinos de sentimiento que exceden cambios de umbral

### 4. Rendimiento y Optimización

- **Tiempo de Procesamiento:** 500ms-2s para agregar 100,000 registros de sentimientos
- **Requisitos de Datos:** Mínimo 100 puntos de datos de sentimientos para análisis estadístico confiable
- **Caché:** Las agregaciones temporales se almacenan en caché durante 1 hora para mejorar tiempos de respuesta
- **Escalabilidad:** Maneja millones de registros de sentimientos usando particionamiento basado en tiempo

## Referencias

* Controller: `src/Domain/ArtificialIntelligence/Http/Controllers/EchoIntelProxyController.php:328`
