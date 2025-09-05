# Inteligencia Artificial – Contador de Usuarios Admin

## Endpoint

`GET /api/v1/ia/admin/users/counter`

Devuelve el número total de usuarios de la plataforma por rol que coinciden con los filtros aplicados en el área administrativa de Inteligencia Artificial. El comportamiento y los filtros coinciden con el endpoint Shared Users Counter.

Véase también: docs/ES/IA/Endpoints/PlatformUserCounter.md

---

## Autenticación

Requerida – Token Bearer con capacidad `backoffice`.

---

## Parámetros de Consulta

Acepta los mismos parámetros de filtro que el AI Users Index (y el Shared Users Index). Consulte:

- docs/ES/IA/Endpoints/PlatformUserIndex.md
- docs/ES/Shared/Endpoints/PlatformUserIndex.md

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

- docs/ES/IA/Endpoints/PlatformUserCounter.md
