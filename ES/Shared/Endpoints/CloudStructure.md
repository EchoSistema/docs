# Shared – Estructura de Carpetas y Archivos (Cloud)

## Endpoint

```
GET /api/v1/platform/cloud/structure/{type}
```

## Autenticación

Obligatoria – Bearer {token} con habilidad adecuada

## Encabezados

| Encabezado         | Tipo     | Obligatorio | Descripción |
| ------------------ | -------- | ----------- | ----------- |
| Authorization      | string   | Sí          | Credencial `Bearer {token}`. |
| X-PUBLIC-KEY       | string   | Sí          | Clave pública de la plataforma. |

## Parámetros

### Parámetros de ruta

| Parámetro | Tipo   | Obligatorio | Descripción |
| --------- | ------ | ----------- | ----------- |
| type      | string | Sí          | Tipo de almacenamiento. Valores aceptados: `images`, `files` |

## Ejemplos

### Ejemplo de solicitud (curl)

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <clave>" \
  "https://sandbox.su-dominio.com/api/v1/platform/cloud/structure/files"
```

### Ejemplo de respuesta

```json
{
  "data": {
    "folders": [
      {
        "title": "uploads",
        "path": "files/domain-slug/platform-slug/uploads"
      },
      {
        "title": "documents",
        "path": "files/domain-slug/platform-slug/documents"
      }
    ],
    "files": [
      {
        "title": "readme.txt",
        "path": "files/domain-slug/platform-slug/readme.txt"
      },
      {
        "title": "config.json",
        "path": "files/domain-slug/platform-slug/config.json"
      }
    ],
    "path": "files/domain-slug/platform-slug"
  }
}
```

## Estructura JSON Explicada

| Campo              | Tipo     | Descripción |
| ------------------ | -------- | ----------- |
| data               | object   | Objeto que contiene la estructura de carpetas y archivos |
| data.folders       | array    | Lista de carpetas de primer nivel |
| data.folders[].title | string | Nombre de la carpeta |
| data.folders[].path  | string | Ruta completa de la carpeta en S3 |
| data.files         | array    | Lista de archivos directos (no en subcarpetas) |
| data.files[].title | string | Nombre del archivo |
| data.files[].path  | string | Ruta completa del archivo en S3 |
| data.base_path     | string   | Ruta base del almacenamiento |
| data.path          | string   | Ruta base del almacenamiento (duplicado para compatibilidad) |

## Estado HTTP

- 200: Éxito
- 401: No autenticado
- 403: Prohibido
- 500: Error interno

## Errores

Errores estándar del sistema:

```json
{
  "message": "Error al acceder al almacenamiento"
}
```

## Notas

- El parámetro `type` define el tipo de almacenamiento a listar
- Valores aceptados para `type`: `images` (imágenes) o `files` (archivos)
- El endpoint retorna solo carpetas y archivos de primer nivel
- Los archivos `.echo` se excluyen del listado (uso interno del sistema)
- La ruta base se construye automáticamente basándose en la plataforma actual
- Formato del path: `{type}/{domain-slug}/{platform-slug}`
- No retorna contenido de subcarpetas recursivamente

## Relacionados

- [CloudImages](CloudImages.md) - Estadísticas de imágenes
- [CloudFiles](CloudFiles.md) - Estadísticas de archivos

## Changelog

- 2025-10-21: Endpoint creado con soporte para listado de carpetas y archivos
