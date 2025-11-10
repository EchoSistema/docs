# Artificial Intelligence – Graph-Based Anomaly Detection

## Endpoint

```
POST /api/v1/ai/echointel/risk/anomaly-graph
```

Uses graph neural networks to detect anomalies in network relationships y transaction patterns.

## Autenticación

Requerido – Bearer {token} con middleware `auth:sanctum`

## Encabezados

| Encabezado          | Tipo   | Requerido | Descripción |
| ------------------ | ------ | ----------- | --------- |
| Authorization      | string | Sí         | `Bearer {token}`. |
| X-Customer-Api-Id  | string | Condicional | UUID del tenant (v4). Requerido if not configured on the server. |
| X-Secret           | string | Condicional | 64-caracteres de secreto. Requerido if not configured on the server. |
| Accept-Language    | string | No         | Idioma de respuesta (`en`, `es`, `pt`). Por Defecto: `en`. |
| Content-Tipo       | string | Sí         | `application/json`. |

## Parámetros

> **Note:** Los parámetros aceptan tanto `snake_case` y `camelCase`.

### Cuerpo de la Solicitud

| Parámetro | Tipo | Requerido | Descripción | Significado Empresarial | Por Defecto |
| --------- | ---- | -------- | ----------- | ---------------- | ------- |
| data | array | Sí | Datos de entrada para el análisis. | Datos empresariales a procesar. | - |
| options | object | No | Opciones de configuración del algoritmo. | Parámetros de personalización para el modelo de ML. | `{}` |
| include_metadata | boolean | No | Incluir metadatos de procesamiento en la respuesta. | Agregar información de diagnóstico. | `false` |

## Ejemplos

### Ejemplo de Solicitud (curl)

```bash
curl -X POST \
  -H "Authorization: Bearer <token>" \
  -H "X-Customer-Api-Id: <tenant-uuid>" \
  -H "X-Secret: <secret>" \
  -H "Accept-Language: en" \
  -H "Content-Type: application/json" \
  -d '{
    "data": [
      {"id": "001", "value": 100}
    ],
    "options": {},
    "include_metadata": true
  }' \
  "https://echosistema.online/api/v1/ai/echointel/risk/anomaly-graph"
```

### Ejemplo de Solicitud (Python)

```python
import requests

url = "https://echosistema.online/api/v1/ai/echointel/risk/anomaly-graph"
headers = {
    "Authorization": "Bearer <token>",
    "X-Customer-Api-Id": "<tenant-uuid>",
    "X-Secret": "<secret>",
    "Accept-Language": "en",
    "Content-Type": "application/json"
}
payload = {
    "data": [{"id": "001", "value": 100}],
    "options": {},
    "include_metadata": True
}

response = requests.post(url, headers=headers, json=payload)
result = response.json()
```

## Respuesta

### Éxito `200 OK`

```json
{
  "results": [
    {
      "id": "001",
      "prediction": 0.85,
      "confidence": 0.92
    }
  ],
  "metadata": {
    "model_version": "v2.1.0",
    "processing_time_ms": 145
  }
}
```

### Error `400 Bad Request`

```json
{
  "error": "Invalid parameters",
  "message": "The data field is required and must contain at least one record."
}
```

### Error `422 Unprocessable Entity`

```json
{
  "error": "Validation failed",
  "message": "Invalid data format",
  "details": {
    "data.0.value": "The value field must be a number."
  }
}
```

## Estructura JSON

| Campo | Tipo | Descripción | Significado Empresarial |
| ----- | ---- | ----------- | ---------------- |
| `results` | array | Arreglo de resultados del análisis. | Salida procesada para cada registro de entrada. |
| `results[].id` | string | Identificador del registro. | Vincula la salida con el registro de entrada. |
| `results[].prediction` | float | Puntuación de predicción (0-1). | Puntuación de confianza de la salida del modelo. |
| `metadata` | object | Metadatos de procesamiento. | Información de diagnóstico y versionado. |
| `metadata.model_version` | string | Versión del modelo de IA utilizada. | Para reproducibilidad y seguimiento. |
| `metadata.processing_time_ms` | integer | Duración del procesamiento en milisegundos. | Métrica de rendimiento. |

## Cómo se Calcula

Uses graph neural networks to detect anomalies in network relationships y transaction patterns.

### Algoritmo Principal

The system employs advanced machine learning y statistical techniques tailored for risk assessment y fraud detection:

- **Preprocesamiento de Datos:** Limpieza, normalización y extracción de características
- **Selección del Modelo:** Selección automática del algoritmo óptimo basado en las características de los datos
- **Predicción/Análisis:** Aplicación del modelo entrenado para generar conocimientos
- **Post-Procesamiento:** Formateo de resultados y aplicación de reglas de negocio

### Pasos de Procesamiento

1. **Validación de Entrada:** Verificar formato de datos, tipos y restricciones empresariales
2. **Ingeniería de Características:** Extraer y transformar características relevantes
3. **Inferencia del Modelo:** Aplicar modelos de ML para generar predicciones/clasificaciones
4. **Agregación de Resultados:** Compilar y formatear resultados con metadatos
5. **Aseguramiento de Calidad:** Validar salida contra rangos y restricciones esperados

### Rendimiento

- **Tiempo de Procesamiento:** 100-500ms para cargas útiles típicas (1,000-10,000 registros)
- **Rendimiento:** 50-100 solicitudes por minuto por tenant
- **Precisión:** Dependiente del modelo, típicamente 85-95% en conjuntos de validación
- **Requisitos de Datos:** Varía por Endpoint, mínimo 100-1,000 registros históricos para entrenamiento

## Estado HTTP

| Código | Descripción |
|------|-------------|
| 200  | Éxito - Análisis completado exitosamente |
| 400  | Bad Request - Parámetros inválidos o campos requeridos faltantes |
| 401  | Unauthorized - Token de autenticación inválido o faltante |
| 403  | Forbidden - Permisos insuficientes o tenant inválido |
| 422  | Unprocessable Entity - Errores de validación en datos de entrada |
| 429  | Too Many Requests - Límite de tasa excedido |
| 500  | Internal Server Error - Error del servicio de IA o tiempo de espera agotado |
| 503  | Service Unavailable - Servicio de IA temporalmente No disponible |

## Errores

**Campos Requeridos Faltantes:**
```json
{
  "error": "Validation failed",
  "message": "Required fields missing",
  "details": {
    "data": "The data field is required and must be an array."
  }
}
```

**Formato de Datos Inválido:**
```json
{
  "error": "Invalid format",
  "message": "Data format does not match expected schema.",
  "expected_format": "Array of objects with required fields"
}
```

**Error del Servicio de IA:**
```json
{
  "error": "Service error",
  "message": "Failed to process request due to AI service error.",
  "retry_after": 60
}
```

## Notas

### Mejores Prácticas

- **Calidad de Datos:** Asegurar que los datos de entrada sean limpios, completos y representativos
- **Tamaño de Lote:** Optimizar tamaños de lote (1,000-10,000 registros) para el mejor rendimiento
- **Manejo de Errores:** Implementar lógica de reintento con retroceso exponencial para errores transitorios
- **Monitoreo:** Rastrear tiempos de procesamiento y métricas de precisión en producción

### Optimización de Rendimiento

- Usar procesamiento por lotes para conjuntos de datos gryes
- Cachear análisis solicitados frecuentemente cuyo sea apropiado
- Minimizar tamaño de carga útil excluyendo campos innecesarios
- Aprovechar compresión para solicitudes gryes (gzip codificación)

### Consideraciones de Seguridad

- Todos los datos están encriptados en tránsito (TLS 1.3) y en reposo
- Las claves API y secretos deben rotarse cada 90 días
- Los registros de auditoría se mantienen por 12 meses
- Las políticas de retención de datos cumplen con GDPR y regulaciones regionales

## Preguntas Frecuentes

### Q: ¿Qué tan precisas son las predicciones/análisis?
**A:** La precisión varía según el Endpoint y la calidad de los datos. La mayoría de los modelos alcanzan 85-95% de precisión en conjuntos de datos de validación. La precisión mejora con datos de entrada de mayor calidad y conjuntos de datos de entrenamiento más gryes. Monitorear la puntuación de `confidence` en las respuestas para confiabilidad por predicción.

### Q: ¿Cuál es el tamaño máximo de carga útil?
**A:** El tamaño máximo de solicitud es 20MB (~250,000 registros dependiendo del conteo de campos). Para conjuntos de datos más gryes, use procesamiento por lotes o contacte a soporte para opciones de procesamiento masivo.

### Q: ¿Con qué frecuencia se reentrenan los modelos?
**A:** Los modelos se reentrenan mensualmente con datos frescos o cuyo se detecta degradación significativa de precisión. El reentrenamiento personalizado de modelos puede solicitarse a través de soporte.

### Q: ¿Puedo usar este Endpoint en aplicaciones en tiempo real?
**A:** Sí, los tiempos de respuesta típicos son 100-500ms. Para casos de uso en tiempo real de alto rendimiento (>1,000 req/min), contacte a soporte para planificación de capacidad dedicada.

### Q: ¿Cómo se maneja la privacidad de los datos?
**A:** Todos los datos de clientes están estrictamente aislados por tenant. Los datos nunca se comparten entre tenants ni se usan para entrenamiento de modelos entre tenants. Cumplimos con GDPR, CCPA y regulaciones específicas de la industria. Los datos se retienen por 90 días a menos que se especifique lo contrario.

### Q: ¿Qué sucede si el servicio de IA No está disponible?
**A:** El sistema devuelve un 503 estado con encabezado `retry_after` indicyo cuándo reintentar. Implemente lógica de reintento con retroceso exponencial (retraso inicial: 1s, máx: 60s). El SLA de disponibilidad del servicio es 99.9% mensual.

## Guías Comerciales

### Playbook 1: Detect fraudulent transactions in real-time
**Objetivo:** Aprovechar conocimientos de IA para lograr resultados empresariales medibles.

**Implementación:**
1. Recopilar y preparar datos históricos para análisis
2. Enviar datos al Endpoint con configuración apropiada
3. Analizar resultados e identificar objetivos de alta prioridad
4. Implementar acciones empresariales basadas en conocimientos
5. Monitorear rendimiento e iterar en estrategia

**Resultados Esperados:**
- 20-40% mejora en métricas empresariales clave
- Costos operacionales reducidos y eficiencia mejorada
- Toma de decisiones basada en datos
- ROI medible dentro de 3-6 meses

### Playbook 2: Assess credit risk for lending decisions
**Objetivo:** Optimizar procesos empresariales usyo conocimientos predictivos.

**Implementación:**
1. Identificar métricas clave y criterios de éxito
2. Integrar Endpoint en flujos de trabajo existentes
3. Usar predicciones para priorizar acciones
4. A/B test enfoques impulsados por IA vs tradicionales
5. Escalar estrategias exitosas en toda la organización

**Resultados Esperados:**
- 15-30% aumento en eficiencia
- Asignación de recursos mejorada
- Ciclos de decisión más rápidos
- Ventaja competitiva mediante adopción de IA

### Playbook 3: Identify suspicious account behavior
**Objetivo:** Impulsar crecimiento de ingresos mediante optimización potenciada por IA.

**Implementación:**
1. Definir métricas de impacto en ingresos
2. Implementar conocimientos de IA en canales de cara al cliente
3. Personalizar experiencias basadas en predicciones
4. Rastrear conversión y aumento de ingresos
5. Refinar continuamente basado en retroalimentación

**Resultados Esperados:**
- 10-25% aumento de ingresos
- Puntuaciones de satisfacción del cliente más altas
- Tasas de conversión mejoradas
- Relaciones con clientes más fuertes

### Playbook 4: Prevent financial losses from fraud
**Objetivo:** Lograr excelencia operacional mediante IA.

**Implementación:**
1. Establecer métricas de línea base
2. Integrar conocimientos de IA en operaciones diarias
3. Automatizar toma de decisiones repetitivas
4. Monitorear KPIs y ajustar umbrales
5. Compartir aprendizajes entre equipos

**Resultados Esperados:**
- 25-50% reducción en esfuerzo manual
- Precisión y consistencia mejoradas
- Tiempo hasta conocimiento más rápido
- Procesos escalables

## Relacionado

- Los endpoints relacionados se listarán aquí según la categoría

## Referencias

* Controlador: `src/Domain/ArtificialIntelligence/Http/Controllers/EchoIntelProxyController.php:356`
