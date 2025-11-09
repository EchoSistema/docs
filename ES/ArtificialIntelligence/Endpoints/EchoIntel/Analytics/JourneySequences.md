# Inteligencia Artificial – Sequências de Jornada

## Endpoint

```
POST /api/v1/ai/echointel/analytics/journey-sequences
```

Sequências de Jornada identificando padrões.

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

El análisis de secuencias de recorrido utiliza algoritmos de minería de patrones secuenciales para descubrir patrones de comportamiento de clientes frecuentes y significativos:

### 1. Minería de Patrones Secuenciales

El sistema emplea algoritmos avanzados de descubrimiento de patrones:

- **Minería Basada en Apriori:** Usa algoritmo Apriori secuencial para encontrar subsecuencias frecuentes que cumplen umbral mínimo de soporte
- **Algoritmo Prefix Span:** Mina eficientemente patrones secuenciales usando metodología de crecimiento de patrones para grandes conjuntos de datos
- **Filtrado Basado en Restricciones:** Aplica restricciones de brecha, ventanas de tiempo y filtros de longitud mínima/máxima

### 2. Proceso de Descubrimiento de Patrones

- **Paso 1:** Convertir datos de recorrido sin procesar en secuencias ordenadas de eventos con timestamps
- **Paso 2:** Minar secuencias frecuentes usando soporte mínimo (predeterminado: 1% de todos los recorridos)
- **Paso 3:** Identificar secuencias cerradas (patrones maximales que subsumen patrones más pequeños)
- **Paso 4:** Clasificar patrones por métricas de soporte, confianza y elevación

### 3. Análisis de Secuencias

- **Análisis de Frecuencia:** Calcular soporte absoluto y relativo para cada patrón descubierto
- **Patrones Temporales:** Identificar patrones basados en tiempo (tendencias horarias, diarias, estacionales en secuencias)
- **Detección de Divergencia:** Encontrar secuencias que diferencian a los conversores de los no conversores

### 4. Rendimiento y Optimización

- **Tiempo de Procesamiento:** 300-600ms para 100,000 secuencias de recorrido
- **Requisitos de Datos:** Mínimo 1,000 recorridos para descubrimiento significativo de patrones
- **Escalabilidad:** Minería paralela para conjuntos de datos que exceden 1 millón de secuencias
- **Poda de Patrones:** Elimina automáticamente patrones redundantes y estadísticamente insignificantes

## Referencias

* Controller: `src/Domain/ArtificialIntelligence/Http/Controllers/EchoIntelProxyController.php:308`
