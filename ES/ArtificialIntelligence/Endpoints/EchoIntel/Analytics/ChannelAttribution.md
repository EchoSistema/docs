# Inteligencia Artificial – Atribuição de Canais

## Endpoint

```
POST /api/v1/ai/echointel/analytics/channel-attribution
```

Atribuição de Canais utilizando modelos de atribuição.

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

El sistema de atribución de canales utiliza múltiples modelos de atribución para asignar crédito por conversiones a través de puntos de contacto de marketing:

### 1. Modelos de Atribución

El sistema soporta cinco modelos estándar de atribución:

- **Atribución Primer Toque:** Asigna 100% del crédito al primer punto de contacto en el recorrido del cliente
- **Atribución Último Toque:** Asigna 100% del crédito al punto de contacto final antes de la conversión
- **Atribución Lineal:** Distribuye el crédito equitativamente entre todos los puntos de contacto del recorrido
- **Atribución Decaimiento Temporal:** Asigna crédito exponencialmente creciente a puntos de contacto más cercanos a la conversión
- **Atribución Valor Shapley:** Usa teoría de juegos para calcular distribución justa de crédito basada en contribución marginal

### 2. Proceso de Cálculo

- **Paso 1:** Agregar rutas de recorrido del cliente de todos los eventos de conversión y no conversión
- **Paso 2:** Para cada modelo, calcular pesos de atribución basados en posiciones y timestamps de puntos de contacto
- **Paso 3:** Aplicar pesos a valores de conversión para determinar contribución del canal
- **Paso 4:** Normalizar resultados para asegurar que la atribución total sea 100% para cada ruta de conversión

### 3. Rendimiento y Optimización

- **Tiempo de Procesamiento:** 150-300ms para 10,000 rutas de recorrido
- **Requisitos de Datos:** Mínimo 100 recorridos completos de clientes para atribución confiable
- **Escalabilidad:** Maneja hasta 1 millón de puntos de contacto por solicitud usando computación distribuida

### 4. Selección de Modelo

- **Valor Shapley** se recomienda para la atribución estadísticamente más rigurosa pero tiene mayor costo computacional (O(n!))
- **Decaimiento Temporal** proporciona un buen balance entre precisión y rendimiento para la mayoría de casos de uso
- **Primer/Último Toque** son útiles para comparaciones de línea base y escenarios de atribución simples

## Referencias

* Controller: `src/Domain/ArtificialIntelligence/Http/Controllers/EchoIntelProxyController.php:298`
