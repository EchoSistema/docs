# IA – Usuarios Admin (Índice)

## Endpoint

`GET /api/v1/ia/admin/users`

Lista usuarios de la plataforma con filtros por rol, ocupación y área, en el área administrativa de IA.

---

## Autenticación

Requerida – Token Bearer con habilidad `backoffice`.

---

## Encabezados

| Encabezado | Tipo | Requerido | Descripción |
| ---------- | ---- | --------- | ----------- |
| Authorization | `string` | Sí | `Bearer {token}`. |
| X-PUBLIC-KEY | `string` | Sí | Clave pública de la plataforma. |
| Accept-Language | `string` | Recomendado | Idioma para campos traducibles (ej.: `pt-BR`, `en`, `es`). |

---

## Parámetros de Consulta

Acepta los mismos filtros documentados en Shared Platform User Index. Consulte:

- docs/ES/Shared/Endpoints/PlatformUserIndex.md

Parámetros soportados incluyen (no exhaustivo): `role`, `roles[]`, `role_id`, `role_name`, `user_name`, `user_email`, `user_uuid`, `job_occupation` (y variaciones), `occupation_area` (y variaciones), `has_job_occupation`, `has_occupation_area`, `no_paginate`, `per_page`.

> Los parámetros pueden enviarse en `snake_case` (canónico) y variantes `camelCase`/`kebab-case`.

---

## Respuesta

Idéntica a la respuesta de Shared Platform User Index. Ejemplo y estructura detallada:

- docs/ES/Shared/Endpoints/PlatformUserIndex.md

---

## Estados HTTP

- 200: OK
- 401: No autorizado
- 403: Prohibido
- 422: Entidad no procesable

---

## Notas

- Devuelve solo usuarios con roles inferiores al rol mínimo del usuario autenticado en la plataforma actual.
- Cuando `has_job_occupation` o `has_occupation_area` son verdaderos, los títulos se cargan según `Accept-Language`.
- Con `no_paginate=true`, devuelve todos los resultados sin metadatos de paginación.

---

## Relacionados

- docs/ES/IA/Endpoints/PlatformUserCounter.md
- docs/ES/IA/Endpoints/PlatformUserShow.md
- docs/ES/IA/Endpoints/PlatformUserStore.md
