# Backoffice – Logs de la Aplicación

## Endpoints

```
GET /api/v1/backoffice/logs              # lista archivos de log disponibles
GET /api/v1/backoffice/logs/{archivo}    # recupera el contenido (tail) de un log específico
```

## Autenticación y Permisos

- Requiere token Sanctum con habilidad `backoffice`.
- Gate `viewLogs`: solo usuarios con permiso `index.all` (ej.: rol *master*) pueden acceder.

## Encabezados

| Encabezado    | Tipo   | Obligatorio | Descripción |
| ------------- | ------ | ----------- | ----------- |
| Authorization | string | Sí          | `Bearer {token}` con habilidad `backoffice`. |
| Accept        | string | No          | `application/json`. |

## Parámetros

### GET /api/v1/backoffice/logs
Sin parámetros.

### GET /api/v1/backoffice/logs/{archivo}
| Parámetro | Tipo   | Obligatorio | Descripción |
| --------- | ------ | ----------- | ----------- |
| archivo   | string | Sí          | Prefijo del log sin la extensión `.log`. Ej.: `order`, `authentication`, `logout`, `log` (mapea a `laravel.log`). |
| lines     | int    | No          | Cantidad máxima de líneas retornadas después de combinar todos los archivos coincidentes (default 200, máximo 500). |

## Ejemplos de Solicitud

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  https://sandbox.echosistema.app/api/v1/backoffice/logs

curl -X GET \
  -H "Authorization: Bearer <token>" \
  "https://sandbox.echosistema.app/api/v1/backoffice/logs/laravel.log?lines=100"
```

## Ejemplos de Respuesta

### Lista de logs
```json
{
  "data": [
    {
      "file": "laravel.log",
      "size": 1048576,
      "size_human": "1.00 MB",
      "last_modified": "2025-11-19T18:00:00+00:00"
    }
  ]
}
```

### Lectura de log
```json
{
  "data": {
    "query": "order",
    "files": [
      {
        "file": "order.log",
        "size": 34567,
        "size_human": "33.8 KB",
        "last_modified": "2025-11-19T18:00:00+00:00"
      },
      {
        "file": "order-2025-11-18.log",
        "size": 12345,
        "size_human": "12.1 KB",
        "last_modified": "2025-11-18T23:00:00+00:00"
      }
    ],
    "lines": [
      {
        "file": "order.log",
        "timestamp": "2025-11-19 18:00:01",
        "environment": "testing",
        "type": "INFO",
        "name": "Ewerton Daniel",
        "message": "Ewerton Daniel successfully authenticated.",
        "context": { "model": "Domain\\Shared\\Models\\User", "id": 24 }
      },
      {
        "file": "order-2025-11-18.log",
        "timestamp": "2025-11-18 23:59:59",
        "environment": "testing",
        "type": "ERROR",
        "message": "Falló",
        "context": { "details": "..." }
      }
    ]
  }
}
```

## Estado HTTP

- 200: OK (retorna arrays vacíos si archivo no encontrado)
- 401: Token inválido o ausente
- 403: Sin permiso `index.all`

## Notas

- Solo se exponen archivos en `storage/logs`.
- **Si archivo no encontrado**: retorna HTTP 200 con `files: []` y `lines: []` en lugar de error 404
- Informe solo el prefijo del archivo (sin `.log`). El endpoint busca todos los archivos que comienzan con ese prefijo (ej.: `order`, `order-2025-11-17.log`) y hace el *merge* de las líneas.
- Cada línea se convierte en un objeto con `timestamp`, `environment`, `type`, `name` (extracto entre `:` y `successfully`), `message` y `context` (cuando el log está en formato estándar Laravel). Si la línea no coincide con el patrón, el campo `line` se devuelve con el contenido sin procesar.
- El contenido retornado es un *tail* consolidado; utilice `lines` para definir el número máximo de líneas agregadas.
- El valor `log` se convierte automáticamente en `laravel.log` por retrocompatibilidad.

## Changelog

- 2025-11-19: Endpoint creado para lectura de logs en backoffice.
