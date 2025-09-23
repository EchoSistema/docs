# Inteligencia Artificial – Contador de Usuarios Admin

## Endpoint

`GET /api/v1/ia/admin/users/counter`

Devuelve el número total de usuarios de la plataforma por rol que coinciden con los filtros aplicados en el área administrativa de Inteligencia Artificial. El comportamiento y los filtros coinciden con el endpoint Shared Users Counter.

Véase también: docs/domain/shared/http/controllers/platform/platform-user-controller.counter.md

---

## Autenticación

Requerida – Token Bearer con capacidad `backoffice`.

---

## Parámetros de Consulta

Acepta los mismos parámetros de filtro que el AI Users Index (y el Shared Users Index). Consulte:

- docs/domain/artificial-intelligence/http/controllers/platform-user-controller.index.md
- docs/domain/shared/http/controllers/platform/platform-user-controller.index.md

---

## Respuesta

Idéntica a la respuesta de Shared Users Counter. Ejemplo:

```json
{
  "counter": {
    "admin": {
      "role_id": 2,
      "name": "admin",
      "localized_name": "Administrator",
      "total": 10
    }
  }
}
```

Para más detalles, consulte:

- docs/domain/shared/http/controllers/platform/platform-user-controller.counter.md
