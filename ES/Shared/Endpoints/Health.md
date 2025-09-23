# Shared – Endpoint de Healthcheck

## Endpoint

`GET /api/v1/health`

Healthcheck simple para balanceadores y monitoreo.

---

## Autenticación

No requiere token Bearer. Requiere cabecera de plataforma.

### Cabeceras Requeridas

| Cabecera | Tipo | Descripción |
| -------- | ---- | ----------- |
| `X-PUBLIC-KEY` | `string` | Clave pública de la plataforma, necesaria para autenticar y recibir respuestas. |
| `Accept-Language` | `string` | Localidad opcional para mensajes. |

---

## Respuesta de Ejemplo

```json
{
  "success": true
}
```

---

## Notas

* Nombre de la ruta: `api.v1.health`.
* La ausencia o invalidez de `X-PUBLIC-KEY` es rechazada por el middleware `platform` (401/403 según configuración).
* Localización: los mensajes de respuesta pueden seguir `Accept-Language`.
