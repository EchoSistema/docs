# Shared – Verificación de Salud

## Endpoints

Cada dominio tiene su propia ruta de verificación de salud:

```
GET /api/v1/advertising/health
GET /api/v1/affiliate/health
GET /api/v1/articles/health
GET /api/v1/ai/health
GET /api/v1/bio/health
GET /api/v1/service/health              # Brick
GET /api/v1/cms/health
GET /api/v1/company/health
GET /api/v1/ecommerce/health
GET /api/v1/find-a-job/health
GET /api/v1/footprints/health
GET /api/v1/health-care/health
GET /api/v1/its-checked/health
GET /api/v1/learn/health
GET /api/v1/link-list/health
GET /api/v1/public/health               # Microservices
GET /api/v1/news/health
GET /api/v1/payment-hub/health
GET /api/v1/pigeons/health
GET /api/v1/real-estate/health
GET /api/v1/rent/health
GET /api/v1/complaints/health           # ReputationBook
GET /api/v1/university/health
```

## Autenticación

Ninguna

## Encabezados

| Encabezado     | Tipo | Requerido | Descripción |
| ---------------- | ------ | -------- | ----------- |
| Authorization    | string | No | Credencial `Bearer {token}`. |
| X-PUBLIC-KEY     | string | Sí      | Clave pública de la plataforma. |
| Accept-Language  | string | No       | Locale IETF (ej.: `pt-BR`, `en`, `es`). |

## Parámetros

Sin parámetros obligatorios. Opcionalmente, proporcione `pbk=<uuid>` en la cadena de consulta si no puede enviar encabezados.

## Ejemplos

### Ejemplo de solicitud (curl)

```bash
# Verificar salud de un dominio específico
curl -X GET \
  -H "X-PUBLIC-KEY: 123e4567-e89b-12d3-a456-426614174000" \
  -H "Accept-Language: es" \
  "https://sandbox.your-domain.com/api/v1/real-estate/health"
```

### Ejemplo de respuesta

```json
{
  "data": {
    "domain": "RealEstate",
    "slug": "real-estate",
    "status": "ok",
    "checked_at": "2025-11-19T18:43:00+00:00",
    "routes": {
      "file": "src/Domain/RealEstate/routes/api.php",
      "exists": true,
      "last_modified": "2025-11-18T20:01:00+00:00"
    }
  }
}
```

## Estructura JSON Explicada

| Campo | Tipo | Descripción |
| ----------- | ------- | ----------- |
| data | object | Información de salud del dominio. |
| data.domain | string | Nombre del dominio (carpeta en `src/Domain`). |
| data.slug | string | Slug utilizado en la ruta (`Str::kebab(domain)`). |
| data.status | string | `ok` cuando existe el archivo de rutas, `missing-routes` en caso contrario. |
| data.checked_at | string | Marca de tiempo de la verificación actual (ISO 8601). |
| data.routes.file | string | Ruta relativa al archivo `routes/api.php`. |
| data.routes.exists | boolean | Indica si el archivo existe. |
| data.routes.last_modified | string\|null | Marca de tiempo de la última actualización (ISO 8601). |

## Estados HTTP

- 200: OK
- 201: Creado
- 400: Solicitud inválida
- 401: No autorizado
- 403: Prohibido
- 404: No encontrado
- 422: Entidad no procesable
- 429: Demasiadas solicitudes
- 500: Error interno del servidor

## Errores

```json
{
  "message": "Mensaje de error"
}
```

## Notas

- Cada dominio tiene su propia ruta de verificación de salud registrada directamente en su respectivo `routes/api.php`.
- El endpoint verifica si el archivo de rutas del dominio existe y devuelve la hora de la última modificación.
- Ya no existe un endpoint agregador global `/health`. Cada dominio debe verificarse individualmente.
- El endpoint no requiere inicio de sesión, pero requiere la clave pública (`X-PUBLIC-KEY` o `pbk`) para identificar el tenant.

## Relacionados

- Ver otros endpoints de la API Shared

## Changelog

- 2025-11-19: Actualizado para reflejar las rutas individuales de verificación de salud por dominio y eliminación del agregador global.
- 2025-10-16: Documentación inicial
