# ReputationBook – Métricas de reclamaciones

## Resumen de contadores

### Endpoint

```
GET /api/v1/complaint-book/complaints/counter
```

Devuelve el total de reclamaciones y la distribución por estado según los filtros enviados.

### Autenticación

Obligatoria – `Bearer {token}` con habilidades `backoffice` y `domain:reputationbook`.

### Parámetros de consulta

Acepta los mismos filtros que `ComplaintFilterRequest` (estado, empresa, periodos, etc.).

### Ejemplo de respuesta (200)

```json
{
  "data": {
    "total": 128,
    "status": {
      "open": 64,
      "in_progress": 40,
      "resolved": 24
    },
    "month": 12
  }
}
```

> La clave `month` solo aparece cuando se envía el filtro `month`.

### Códigos HTTP

- 200: Datos retornados.
- 401/403: Falta de permisos.

---

## Informe por intervalo

### Endpoint

```
GET /api/v1/complaint-book/complaints/report/{interval}
```

Genera una serie temporal basada en los últimos cambios de estado.

### Parámetros de ruta

| Parámetro | Valores aceptados |
| --------- | ---------------- |
| `interval` | `annual`, `semi-annual`, `tri-monthly`, `bi-monthly`, `monthly`, `weekly` (cualquier otro valor toma el año en curso). |

### Respuesta

```json
{
  "success": true,
  "counter": {
    "2025-06": {
      "open": 12,
      "resolved": 4
    },
    "2025-07": {
      "open": 8,
      "in_progress": 6,
      "resolved": 9
    }
  },
  "interval": 6
}
```

- `counter`: mapa `YYYY-MM` → totales por estado (solo se considera el último evento de cada reclamación dentro del periodo).
- `interval`: número de meses que cubre el recorte.

### Códigos HTTP

- 200: Informe generado.
- 401/403: Falta de permisos.

