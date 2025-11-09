# Inteligencia Artificial – Propensão a Upgrade de Plano

## Endpoint

```
POST /api/v1/ai/echointel/propensity/upgrade-plan
```

Propensão a Upgrade de Plano utilizando modelos predictivos de propensión.

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

* Controller: `src/Domain/ArtificialIntelligence/Http/Controllers/EchoIntelProxyController.php:187`
