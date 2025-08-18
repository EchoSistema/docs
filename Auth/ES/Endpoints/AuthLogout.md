# Auth – Endpoint Cerrar Sesión

## Endpoint

`POST /auth/logout`

Finaliza la sesión del usuario invalidando el token de acceso.

---

## Autenticación

Requiere un token Bearer válido.

### Encabezados Requeridos

| Encabezado | Tipo | Descripción |
| ---------- | ---- | ----------- |
| **Authorization** | `string` | `Bearer <token>` requerido. |
| **X-PUBLIC-KEY** | `string` | Clave pública de la plataforma necesaria para autenticarse y recibir respuestas. |
| **Accept-Language** | `string` | Locale para mensajes de respuesta. |

---

## Respuesta de Ejemplo

```json
{
  "success": true,
  "message": "Usuario :name cerró sesión correctamente."
}
```

---

## Notas

* El mensaje de respuesta se traduce según `Accept-Language`.

