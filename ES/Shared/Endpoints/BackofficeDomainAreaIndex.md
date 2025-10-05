# Shared – Listar Áreas de Dominio (Backoffice)

## Endpoint

```
GET /api/v1/backoffice/domain-areas
```

## Autenticación

Obligatoria – Bearer {token} con permiso `index.all`

## Encabezados

| Encabezado        | Tipo     | Obligatorio | Descripción |
| ----------------- | -------- | ----------- | ----------- |
| Authorization     | string   | Sí          | Credencial `Bearer {token}`. |
| X-PUBLIC-KEY      | string   | Sí          | Clave pública de la plataforma. |

## Parámetros

Este endpoint no acepta parámetros de ruta o consulta.

## Ejemplos

### Ejemplo de solicitud (curl)

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <clave>" \
  "https://sandbox.tu-dominio.com/api/v1/backoffice/domain-areas"
```

### Ejemplo de respuesta

```json
{
  "data": [
    {
      "uuid": "550e8400-e29b-41d4-a716-446655440000",
      "name": "Biografía",
      "slug": "bio",
      "is_active": true
    },
    {
      "uuid": "660e8400-e29b-41d4-a716-446655440001",
      "name": "Salud",
      "slug": "healthcare",
      "is_active": true
    }
  ]
}
```

## Estructura JSON Explicada

| Campo           | Tipo     | Descripción |
| --------------- | -------- | ----------- |
| data[]          | array    | Lista de áreas de dominio disponibles en el sistema |
| data[].uuid     | string   | Identificador único universal del área de dominio |
| data[].name     | string   | Nombre del área de dominio |
| data[].slug     | string   | Slug del área de dominio (identificador textual único) |
| data[].is_active| boolean  | Indica si el área de dominio está activa en el sistema |

## Estado HTTP

- 200: Éxito
- 401: No autenticado
- 403: Prohibido (sin permiso `index.all`)
- 500: Error interno

## Errores

### 403 Forbidden
El usuario no tiene el permiso necesario:

```json
{
  "message": "Forbidden"
}
```

### 401 Unauthorized
Token ausente o inválido:

```json
{
  "message": "Unauthenticated."
}
```

## Notas

- Este endpoint devuelve todas las áreas de dominio registradas en el sistema, incluyendo las inactivas
- Las áreas de dominio se utilizan para organizar plataformas, categorías y tipos de elementos
- El campo `slug` puede usarse para identificar áreas de dominio específicas programáticamente
- La verificación de permisos se realiza a través del método `checkPermissions('index.all')` del controlador
- Solo se devuelven los campos `uuid`, `name`, `slug` e `is_active` (campos seleccionados explícitamente)

## Relacionados

- [Listar Plataformas](./BackofficePlatformIndex.md)
- [Listar Categorías por Dominio](./CategoryGroupIndex.md)

## Changelog

- 2025-10-05: Documentación inicial creada
