# ReputationBook – Panel de Quejas

## Descripción

Este documento describe la estructura de métricas almacenadas en MongoDB para los paneles de quejas de ReputationBook. Los datos se almacenan en la colección `dashboards` de la base de datos `reputation-book`.

## Modelo

**Namespace**: `Domain\ReputationBook\Models\Mongo\ComplaintBookDashboard`

**Colección**: `dashboards`

**Base de datos**: `reputation-book`

## Estructura del Campo `metrics`

El campo `metrics` es un objeto JSON que contiene diferentes tipos de métricas agrupadas por categorías. Cada métrica tiene su propia estructura de datos.

---

## Métricas Disponibles

### 1. Quejas por Estado (`complaint_by_status`)

Métrica que muestra la distribución de quejas por estado durante un período específico.

#### Estructura

```json
{
  "complaint_by_status": {
    "period": {
      "type": "week",
      "categories": ["Mon", "Tue", "Wed", "Thu", "Fri", "Sat", "Sun"],
      "startDate": "2024-10-20",
      "endDate": "2024-10-26"
    },
    "summary": {
      "totalComplaints": 7300,
      "averageWeeklySolved": 1500,
      "weeklyChange": "+12%"
    },
    "statusBreakdown": {
      "solved": {
        "total": 1500,
        "percentage": 20.5
      },
      "pending": {
        "total": 4700,
        "percentage": 64.4
      },
      "unresolved": {
        "total": 780,
        "percentage": 10.7
      },
      "spam": {
        "total": 320,
        "percentage": 4.4
      }
    },
    "chartData": {
      "series": [
        {
          "name": "solved",
          "data": [28, 50, 90, 95, 20, 70, 35]
        },
        {
          "name": "pending",
          "data": [80, 60, 70, 30, 45, 20, 80]
        },
        {
          "name": "unresolved",
          "data": [32, 23, 78, 35, 65, 35, 15]
        },
        {
          "name": "spam",
          "data": [60, 25, 80, 25, 15, 40, 15]
        }
      ],
      "categories": ["Mon", "Tue", "Wed", "Thu", "Fri", "Sat", "Sun"]
    }
  }
}
```

#### Campos Detallados

##### `period` - Información del Período

| Campo | Tipo | Descripción |
| ----- | ---- | ----------- |
| type | string | Tipo de período (week, month, year, custom) |
| categories | array | Etiquetas de categorías/fechas del período |
| startDate | string | Fecha de inicio del período (formato: YYYY-MM-DD) |
| endDate | string | Fecha de fin del período (formato: YYYY-MM-DD) |

**Valores posibles para `type`**:
- `week` - Período semanal
- `month` - Período mensual
- `year` - Período anual
- `custom` - Período personalizado

##### `summary` - Resumen General

| Campo | Tipo | Descripción |
| ----- | ---- | ----------- |
| totalComplaints | integer | Total de quejas en el período |
| averageWeeklySolved | integer | Promedio de quejas resueltas por semana |
| weeklyChange | string | Variación porcentual respecto a la semana anterior (ej: "+12%", "-5%") |

##### `statusBreakdown` - Distribución por Estado

Objeto que contiene el recuento y porcentaje para cada estado de queja.

**Estados disponibles**:

| Estado | Campo | Descripción |
| ------ | ----- | ----------- |
| Resueltas | solved | Quejas solucionadas |
| Pendientes | pending | Quejas esperando respuesta/acción |
| No Resueltas | unresolved | Quejas que no fueron resueltas |
| Spam | spam | Quejas marcadas como spam |

**Estructura de cada estado**:

| Campo | Tipo | Descripción |
| ----- | ---- | ----------- |
| total | integer | Total de quejas en este estado |
| percentage | float | Porcentaje con respecto al total |

##### `chartData` - Datos para Gráfico

Estructura preparada para visualización en gráficos (compatible con ApexCharts, Chart.js, etc).

| Campo | Tipo | Descripción |
| ----- | ---- | ----------- |
| series | array | Array de series de datos |
| series[].name | string | Nombre de la serie (estado) en minúsculas |
| series[].data | array | Array de valores numéricos correspondientes a las categorías |
| categories | array | Etiquetas del eje X (días, meses, etc) |

---

## Tipos de Período

### Semanal (`week`)
- **categories**: `["Mon", "Tue", "Wed", "Thu", "Fri", "Sat", "Sun"]`
- **Duración**: 7 días
- **Uso**: Análisis diario de la semana

### Mensual (`month`)
- **categories**: `["Week 1", "Week 2", "Week 3", "Week 4"]` o `["1", "2", ..., "31"]`
- **Duración**: 1 mes
- **Uso**: Análisis semanal o diario del mes

### Anual (`year`)
- **categories**: `["Jan", "Feb", "Mar", ..., "Dec"]`
- **Duración**: 12 meses
- **Uso**: Análisis mensual del año

### Personalizado (`custom`)
- **categories**: Definido por el usuario
- **Duración**: Período personalizado
- **Uso**: Análisis específicos con fechas personalizadas

---

## Estados de Quejas

### Ciclo de Vida

1. **Initiated** - Queja iniciada
2. **Pending** - Esperando análisis/respuesta
3. **Approved** - Aprobada para publicación
4. **Replied** - Empresa respondió
5. **Solved** (Amicably/Legally) - Resuelta
6. **Unresolved** - No resuelta
7. **Spam** - Marcada como spam
8. **Archived** - Archivada
9. **Canceled** - Cancelada

### Agrupación para Panel

- **Solved**: `amicably_resolved`, `legally_resolved`
- **Pending**: `initiated`, `pending`, `approved`, `replied`
- **Unresolved**: `unresolved`
- **Spam**: `spam`

---

## Cálculos y Fórmulas

### Porcentaje por Estado

```
percentage = (total_status / total_complaints) * 100
```

### Variación Semanal

```
weeklyChange = ((current_week - previous_week) / previous_week) * 100
```

Ejemplo:
- Semana Anterior: 1340 resueltas
- Semana Actual: 1500 resueltas
- Variación: `((1500 - 1340) / 1340) * 100 = +11.94%` ≈ `+12%`

### Promedio Semanal

```
averageWeeklySolved = sum(solved_per_week) / total_weeks
```

---

## Casos de Uso

### 1. Gráfico de Área Apilada

Muestra la evolución de cada estado a lo largo del tiempo de forma apilada.

**Datos**: `chartData.series`

**Eje X**: `chartData.categories`

**Eje Y**: Número de quejas

### 2. Gráfico de Pastel/Dona

Muestra la distribución actual por estado.

**Datos**: `statusBreakdown` (usar valores `total` o `percentage`)

**Etiquetas**: Nombres de estados (solved, pending, unresolved, spam)

### 3. Tarjetas de Resumen

Mostrar métricas principales en tarjetas del panel.

**Datos**: `summary`
- Total de Quejas
- Promedio Semanal Resuelto
- Variación de la Semana

### 4. Tabla de Estados

Lista detallada de cada estado con total y porcentaje.

**Datos**: `statusBreakdown`

---

## Ejemplos de Consultas MongoDB

### Buscar Panel de la Semana Actual

```javascript
db.dashboards.findOne({
  "metrics.complaint_by_status.period.type": "week",
  "metrics.complaint_by_status.period.startDate": "2024-10-20",
  "metrics.complaint_by_status.period.endDate": "2024-10-26"
})
```

### Buscar Panel por Plataforma

```javascript
db.dashboards.findOne({
  "platform_id": 1,
  "period": "2024-10-20_2024-10-26"
})
```

### Agregación de Múltiples Plataformas

```javascript
db.dashboards.aggregate([
  {
    $match: {
      "metrics.complaint_by_status.period.type": "week",
      "createdAt": { $gte: ISODate("2024-10-20") }
    }
  },
  {
    $group: {
      _id: "$metrics.complaint_by_status.period.type",
      totalComplaints: { $sum: "$metrics.complaint_by_status.summary.totalComplaints" },
      totalSolved: { $sum: "$metrics.complaint_by_status.statusBreakdown.solved.total" }
    }
  }
])
```

---

## Buenas Prácticas

### 1. Actualización de Datos
- Actualizar métricas diariamente mediante cron/job
- Mantener historial de períodos anteriores
- Usar `updateOrCreate` para evitar duplicados

### 2. Rendimiento
- Indexar por `platform_id`, `company_id` y `period`
- Almacenar datos precalculados
- Evitar cálculos en tiempo real en el frontend

### 3. Versionamiento
- Incluir campo `version` para migración de estructura
- Mantener compatibilidad retroactiva
- Documentar cambios de estructura

### 4. Validación
- Validar que `categories` y `series[].data` tengan el mismo tamaño
- Garantizar que los porcentajes sumen ~100%
- Verificar fechas de inicio/fin del período

---

## Estructura Completa del Documento

```json
{
  "_id": "ObjectId",
  "platform_id": 1,
  "company_id": null,
  "period": "2024-10-20_2024-10-26",
  "metrics": {
    "complaint_by_status": {
      "period": { ... },
      "summary": { ... },
      "statusBreakdown": { ... },
      "chartData": { ... }
    },
    "complaint_by_company": { ... },
    "complaint_resolution_time": { ... },
    "complaint_satisfaction": { ... }
  },
  "createdAt": "2024-10-20T00:00:00.000Z",
  "updatedAt": "2024-10-26T23:59:59.000Z"
}
```

---

## Métricas Futuras (Planificadas)

### `complaint_by_company`
Distribución de quejas por empresa en el período.

### `complaint_resolution_time`
Tiempo promedio de resolución de quejas por estado.

### `complaint_satisfaction`
Índice de satisfacción de usuarios con las resoluciones.

### `complaint_by_category`
Distribución de quejas por categoría de producto/servicio.

### `complaint_by_platform`
Análisis comparativo entre diferentes plataformas.

---

## Relacionados

- [Modelo ComplaintBookDashboard](../Models/Mongo/ComplaintBookDashboard.md)
- [Modelo Complaint](../Models/Complaint.md)
- [Endpoints API del Panel](../Endpoints/BackofficeComplaintBookDashboard.md)

## Changelog

- 2025-10-26: Documentación inicial con estructura de `complaint_by_status`
