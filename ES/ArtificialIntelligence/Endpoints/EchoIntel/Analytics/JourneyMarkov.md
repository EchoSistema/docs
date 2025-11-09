# Inteligencia Artificial – Jornada del Cliente (Markov)

## Endpoint

```
POST /api/v1/ai/echointel/analytics/journey-markov
```

Jornada del Cliente (Markov) con cadeias de Markov.

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

El análisis de recorrido utiliza modelado de cadenas de Markov para entender probabilidades de rutas de clientes y probabilidad de conversión:

### 1. Construcción de Cadena de Markov

El sistema construye una máquina de estados probabilística a partir de datos de recorrido del cliente:

- **Identificación de Estados:** Cada punto de contacto único (canal, página, acción) se convierte en un estado en la cadena de Markov
- **Matriz de Transición:** Calcula probabilidad P(estado_j | estado_i) para todos los pares de estados basado en secuencias de recorrido observadas
- **Estados Absorbentes:** Define estados terminales (conversión, abandono) que los clientes no pueden dejar una vez ingresados

### 2. Cálculo de Probabilidad de Transición

- **Paso 1:** Contar todas las transiciones observadas del estado i al estado j a través de todos los recorridos de clientes
- **Paso 2:** Normalizar por transiciones totales del estado i para obtener P(j|i) = count(i→j) / sum(count(i→*))
- **Paso 3:** Manejar datos dispersos usando suavizado de Laplace (suavizado add-one) para transiciones raras

### 3. Análisis de Recorrido

- **Probabilidad de Conversión:** Calcular probabilidad de alcanzar conversión desde cualquier estado usando análisis de cadena absorbente
- **Longitud de Ruta Esperada:** Calcular número medio de pasos hasta conversión o abandono
- **Rutas Críticas:** Identificar secuencias de alta probabilidad que llevan a conversión usando algoritmo Viterbi

### 4. Rendimiento y Optimización

- **Tiempo de Procesamiento:** 200-400ms para 50,000 secuencias de recorrido
- **Requisitos de Datos:** Mínimo 500 recorridos completos para probabilidades de transición estables
- **Eficiencia de Memoria:** Usa representación de matriz dispersa para espacios de estado grandes (10,000+ estados)

## Referencias

* Controller: `src/Domain/ArtificialIntelligence/Http/Controllers/EchoIntelProxyController.php:303`
