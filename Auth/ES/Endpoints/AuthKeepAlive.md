# Auth – Endpoint Mantener Sesión

## Endpoint

`GET /auth/keep-alive`

Verifica si el token proporcionado es válido y mantiene activa la sesión del usuario.

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
  "message": "Sesión de usuario mantenida activa."
}
```

---

## Notas

* El mensaje de respuesta se traduce según `Accept-Language`.

