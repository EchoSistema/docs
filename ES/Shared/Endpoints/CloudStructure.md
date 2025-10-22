# Shared – Estructura de Almacenamiento en la Nube

## Endpoint

```
GET /api/v1/cloud/structure?folder={folder}
```

## Descripción

Recupera la estructura de carpetas y archivos del almacenamiento en la nube (S3) para una ruta de carpeta específica. Devuelve subcarpetas de primer nivel y archivos junto con sus rutas completas. El endpoint valida que la carpeta solicitada pertenece a la plataforma actual.

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
| folder    | string | Sí          | Ruta completa de la carpeta en S3. Debe contener el slug del dominio y nombre de la plataforma. Formato: `{tipo}/{domain-slug}/{platform-slug}` o cualquier subcarpeta dentro. |

## Ejemplos

### Ejemplo de solicitud (curl)

#### Carpeta raíz de la plataforma
```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <clave>" \
  "https://api.example.com/api/v1/cloud/structure?folder=files/real-estate/mi-plataforma"
```

#### Subcarpeta
```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <clave>" \
  "https://api.example.com/api/v1/cloud/structure?folder=files/real-estate/mi-plataforma/uploads"
```

### Ejemplo de respuesta - Éxito (200)

```json
{
  "data": {
    "folders": [
      {
        "title": "uploads",
        "path": "files/real-estate/mi-plataforma/uploads",
        "file_count": 45,
        "total_size": 15728640,
        "total_size_formatted": "15.00 MB",
        "last_modified": "2025-10-21 14:30:25"
      },
      {
        "title": "documentos",
        "path": "files/real-estate/mi-plataforma/documentos",
        "file_count": 12,
        "total_size": 2097152,
        "total_size_formatted": "2.00 MB",
        "last_modified": "2025-10-20 09:15:10"
      },
      {
        "title": "reportes",
        "path": "files/real-estate/mi-plataforma/reportes",
        "file_count": 8,
        "total_size": 5242880,
        "total_size_formatted": "5.00 MB",
        "last_modified": "2025-10-19 16:45:00"
      }
    ],
    "files": [
      {
        "title": "leeme.txt",
        "path": "files/real-estate/mi-plataforma/leeme.txt",
        "size": 2048,
        "size_formatted": "2.00 KB",
        "mime_type": "text/plain",
        "extension": "txt",
        "last_modified": "2025-10-15 08:30:00",
        "last_modified_timestamp": 1729000200
      },
      {
        "title": "config.json",
        "path": "files/real-estate/mi-plataforma/config.json",
        "size": 1024,
        "size_formatted": "1.00 KB",
        "mime_type": "application/json",
        "extension": "json",
        "last_modified": "2025-10-18 11:20:45",
        "last_modified_timestamp": 1729254045
      }
    ],
    "base_path": "files/real-estate/mi-plataforma",
    "path": "files/real-estate/mi-plataforma"
  }
}
```

### Ejemplo de respuesta - Carpeta vacía (200)

```json
{
  "data": {
    "folders": [],
    "files": [],
    "base_path": "files/real-estate/mi-plataforma/carpeta-vacia",
    "path": "files/real-estate/mi-plataforma/carpeta-vacia"
  }
}
```

## Estructura JSON Explicada

### Objeto Response

| Campo              | Tipo     | Descripción |
| ------------------ | -------- | ----------- |
| data               | object   | Objeto contenedor para la estructura de almacenamiento |

### Objeto data

| Campo              | Tipo     | Descripción |
| ------------------ | -------- | ----------- |
| folders            | array    | Lista de subcarpetas de primer nivel en la ruta solicitada |
| files              | array    | Lista de archivos directamente en la ruta solicitada (no en subcarpetas) |
| base_path          | string   | La ruta de la carpeta solicitada (igual que el parámetro path) |
| path               | string   | La ruta de la carpeta solicitada (igual que `base_path`, mantenido para compatibilidad) |

### Objeto folders[]

| Campo                  | Tipo     | Descripción |
| ---------------------- | -------- | ----------- |
| title                  | string   | Nombre de la carpeta (último segmento de la ruta) |
| path                   | string   | Ruta completa de la carpeta en el almacenamiento S3 |
| file_count             | integer  | Número total de archivos en la carpeta (recursivo) |
| total_size             | integer  | Tamaño total de todos los archivos en bytes |
| total_size_formatted   | string   | Tamaño total legible (ej: "15.00 MB") |
| last_modified          | string\|null | Fecha de última modificación del archivo más reciente (formato Y-m-d H:i:s), null si no hay archivos |

### Objeto files[]

| Campo                    | Tipo     | Descripción |
| ------------------------ | -------- | ----------- |
| title                    | string   | Nombre del archivo con extensión |
| path                     | string   | Ruta completa del archivo en el almacenamiento S3 |
| size                     | integer  | Tamaño del archivo en bytes |
| size_formatted           | string   | Tamaño del archivo legible (ej: "512.00 KB") |
| mime_type                | string   | Tipo MIME del archivo (ej: "image/jpeg", "application/pdf") |
| extension                | string   | Extensión del archivo sin punto (ej: "jpg", "pdf") |
| last_modified            | string   | Fecha de última modificación (formato Y-m-d H:i:s) |
| last_modified_timestamp  | integer  | Timestamp Unix de la última modificación |

## Estado HTTP

- **200 OK**: Estructura recuperada con éxito
- **401 Unauthorized**: Token de autenticación ausente o inválido
- **403 Forbidden**: Permisos insuficientes para acceder al almacenamiento en la nube
- **404 Not Found**: La carpeta no pertenece a la plataforma actual o no existe
- **500 Internal Server Error**: Error al acceder al almacenamiento S3

## Errores

### Carpeta no encontrada o no autorizada (404)

Cuando la carpeta solicitada no contiene el slug del dominio y nombre de la plataforma:

```json
{
  "message": "carpeta no encontrada"
}
```

### Error de acceso al almacenamiento (500)

```json
{
  "message": "Error al acceder al almacenamiento en la nube"
}
```

## Reglas de Negocio

### 1. Validación de Plataforma
- El endpoint valida que la ruta de la carpeta contiene `/{domain-slug}/{platform-slug}`
- Ejemplo: Para la plataforma "Mi Plataforma" en el dominio "real-estate", la ruta debe contener `/real-estate/mi-plataforma`
- Esto impide que los usuarios accedan a carpetas de otras plataformas
- Devuelve 404 si la validación falla

### 2. Formato de la Ruta
- **Ruta completa obligatoria**: El parámetro folder debe ser una ruta S3 completa
- **Ejemplos válidos**:
  - `files/real-estate/mi-plataforma` (carpeta raíz de la plataforma)
  - `files/real-estate/mi-plataforma/uploads` (subcarpeta)
  - `images/real-estate/mi-plataforma/avatars/2024` (subcarpeta anidada)
- **Ejemplos inválidos**:
  - `mi-plataforma` (incompleto)
  - `files/otro-dominio/otra-plataforma` (plataforma diferente)

### 3. Listado de Carpetas
- Devuelve solo subdirectorios de primer nivel de la ruta solicitada
- No hace recursión en carpetas anidadas
- Los nombres de carpetas se extraen de rutas de archivos (carpetas vacías no aparecen si no contienen archivos)

### 4. Filtrado de Archivos
- Solo se incluyen archivos directamente en la ruta solicitada
- Los archivos en subdirectorios se excluyen
- Los archivos `.echo` se excluyen del listado (uso interno del sistema)

### 5. Resultados Vacíos
- Si no existen carpetas en la ruta, el array `folders` estará vacío
- Si no existen archivos en la ruta, el array `files` estará vacío
- Ambos arrays pueden estar vacíos simultáneamente (respuesta válida para carpetas vacías)

### 6. Codificación de URL
- Las rutas de carpetas con caracteres especiales deben estar codificadas en URL
- Ejemplo: `files/real-estate/mi-plataforma/mi carpeta` se convierte en `files/real-estate/mi-plataforma/mi%20carpeta`

## Seguridad

### Aislamiento de Plataforma
El endpoint impone un aislamiento estricto de plataforma:
- Los usuarios solo pueden acceder a carpetas dentro del namespace de su plataforma autenticada
- La validación verifica que la ruta de la carpeta contiene el slug del dominio y nombre de la plataforma
- Los intentos de acceder a carpetas de otras plataformas resultarán en errores 404

### Prevención de Path Traversal
- El mecanismo de validación previene ataques de path traversal
- Los usuarios no pueden usar `..` u otros trucos para escapar de la carpeta de su plataforma

## Consideraciones de Rendimiento

- El endpoint lista todos los archivos en la ruta solicitada para determinar la estructura
- El rendimiento puede variar según el número de archivos en la carpeta
- Carpetas grandes con miles de archivos pueden tardar más en procesarse
- Los resultados no se almacenan en caché
- Considere la paginación para carpetas con muchos elementos (no implementado actualmente)

## Endpoints Relacionados

- [CloudImages](CloudImages.md) - Obtener estadísticas del directorio de imágenes
- [CloudFiles](CloudFiles.md) - Obtener estadísticas del directorio de archivos

## Casos de Uso

### 1. Navegación en Explorador de Archivos
Construir un explorador de archivos jerárquico que permite a los usuarios navegar por su almacenamiento en la nube:

```javascript
// Navegar a la raíz
GET /api/v1/cloud/structure?folder=files/real-estate/mi-plataforma

// Usuario hace clic en la carpeta "uploads"
GET /api/v1/cloud/structure?folder=files/real-estate/mi-plataforma/uploads

// Usuario hace clic en la subcarpeta "2024"
GET /api/v1/cloud/structure?folder=files/real-estate/mi-plataforma/uploads/2024
```

### 2. Widget de Selección de Carpeta
Mostrar carpetas disponibles para carga u organización de archivos:

```javascript
// Obtener carpetas disponibles
GET /api/v1/cloud/structure?folder=files/real-estate/mi-plataforma

// Mostrar carpetas al usuario: "uploads", "documentos", "reportes"
// Usuario selecciona "documentos" para cargar archivo
```

### 3. Panel de Visión General de Almacenamiento
Mostrar estructura de carpetas y contenido en un panel:

```javascript
// Obtener estructura raíz
GET /api/v1/cloud/structure?folder=files/real-estate/mi-plataforma

// Mostrar:
// - Carpetas: uploads (clic para expandir), documentos, reportes
// - Archivos: leeme.txt, config.json
```

### 4. Navegación Breadcrumb
Construir navegación breadcrumb para jerarquía de carpetas:

```javascript
// Ruta actual: files/real-estate/mi-plataforma/uploads/2024/enero
// Breadcrumb: Inicio > uploads > 2024 > enero
// Cada segmento es clicable y llama al endpoint structure
GET /api/v1/cloud/structure?folder=files/real-estate/mi-plataforma              // Inicio
GET /api/v1/cloud/structure?folder=files/real-estate/mi-plataforma/uploads      // uploads
GET /api/v1/cloud/structure?folder=files/real-estate/mi-plataforma/uploads/2024 // 2024
GET /api/v1/cloud/structure?folder=files/real-estate/mi-plataforma/uploads/2024/enero // enero
```

## Notas de Implementación

### Detección de Plataforma
El endpoint detecta automáticamente la plataforma actual del contexto del usuario autenticado usando `App::platform()`. La validación asegura que la carpeta solicitada pertenece a esta plataforma.

### Extracción de Nombre de Carpeta
Los nombres de carpetas se extraen de rutas de archivos. Si una carpeta está completamente vacía (sin archivos a ninguna profundidad), no aparecerá en los resultados.

### Componentes de Ruta
La ruta se divide en componentes para extraer carpetas de primer nivel. Por ejemplo:
- Ruta de solicitud: `files/real-estate/mi-plataforma`
- Archivo: `files/real-estate/mi-plataforma/uploads/documento.pdf`
- Carpeta extraída: `uploads`

## Changelog

- **2025-10-21**: Endpoint creado con soporte para listado de carpetas y archivos
- **2025-10-21**: Agregada validación de plataforma para prevenir acceso entre plataformas
- **2025-10-21**: Parámetro cambiado de `type` a `folder` para soporte de ruta completa
- **2025-10-21**: Agregado campo `base_path` a la estructura de respuesta
- **2025-10-21**: Agregados mensajes de error localizados usando `validation.attributes.folder`
- **2025-10-21**: Agregados metadatos S3 para archivos (size, mime_type, extension, last_modified)
- **2025-10-21**: Agregados metadatos S3 para carpetas (file_count, total_size, last_modified)
