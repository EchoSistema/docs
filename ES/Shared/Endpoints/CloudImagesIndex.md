# Shared – Estadísticas de Almacenamiento de Imágenes (S3)

## Endpoint

```
GET /api/v1/cloud/images
```

## Autenticación

Requerida – Bearer {token}

## Encabezados

| Encabezado       | Tipo   | Requerido | Descripción |
| ---------------- | ------ | --------- | ----------- |
| Authorization    | string | Sí | Credencial `Bearer {token}`. |
| X-PUBLIC-KEY     | string | Sí        | Clave pública de la plataforma. |
| Accept-Language  | string | No        | Locale IETF (ej.: `pt-BR`, `en`, `es`). |

## Parámetros

No se requieren parámetros adicionales. El endpoint retorna estadísticas del directorio de imágenes de la plataforma actual en AWS S3.

## Ejemplos

### Ejemplo de solicitud (curl)

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: es" \
  "https://sandbox.your-domain.com/api/v1/cloud/images"
```

### Ejemplo de respuesta

```json
{
  "data": {
    "path": "images/domain-slug/platform-name",
    "files": 15,
    "directories": 3,
    "total_size": 2621440,
    "total_size_formatted": "2.5 MB",
    "timestamp": "2025-10-21 10:30:45"
  }
}
```

## Estructura JSON Explicada

| Campo | Tipo | Descripción |
| ----------- | ------- | ----------- |
| data        | object  | Estadísticas del directorio de imágenes |
| data.path   | string  | Ruta relativa en el bucket S3 (formato: `images/{domain-slug}/{platform-name-slug}`) |
| data.files  | integer | Número total de archivos (excluyendo .echo) |
| data.directories | integer | Número total de subdirectorios |
| data.total_size | integer | Tamaño total en bytes |
| data.total_size_formatted | string | Tamaño formateado en formato legible (KB, MB, GB, etc) |
| data.timestamp | string | Fecha y hora de la consulta (formato: Y-m-d H:i:s) |

## Estado HTTP

- 200: OK
- 401: No autenticado
- 403: Prohibido
- 500: Error interno (posible fallo de conexión con S3)

## Errores

```json
{
  "message": "Mensaje de error"
}
```

Posibles errores específicos:
- Credenciales S3 inválidas o expiradas
- Permisos insuficientes en el bucket S3
- Timeout de conexión con AWS S3

## Notas

- El endpoint crea automáticamente el archivo `.echo` en S3 si no existe
- Cada consulta agrega una nueva línea al archivo `.echo` con timestamp y estadísticas
- El archivo `.echo` es excluido del conteo de archivos y tamaño
- La estructura de carpetas en S3 se basa en el slug del dominio y nombre de la plataforma actual
- Formato de ruta: `images/{domain-slug}/{platform-name-slug}/`
- El cálculo se realiza de forma recursiva incluyendo todos los subdirectorios

## Relacionados

- [Estadísticas de Archivos](CloudFilesIndex.md)

## Changelog

- 2025-10-21: Documentación inicial
