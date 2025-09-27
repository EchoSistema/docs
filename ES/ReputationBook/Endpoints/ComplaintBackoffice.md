# ReputationBook – Gestión de reclamaciones (backoffice)

Todas las rutas siguientes requieren autenticación `Bearer {token}` (`auth:sanctum`) con las habilidades `backoffice` y `domain:reputationbook`.

## Listar reclamaciones de la plataforma

```
GET /api/v1/complaint-book/complaints
```

- Acepta los filtros de `ComplaintFilterRequest`.
- Devuelve `ComplaintCollection` incluyendo usuarios, empresas y textos.
- Soporta `no_paginate=true` para obtener una lista limitada (`limit`).

## Crear reclamación en nombre de un usuario

```
POST /api/v1/complaint-book/complaints/{user}
```

| Parámetro | Tipo | Descripción |
| --------- | ---- | ----------- |
| `user` | string | ID o UUID del usuario representado. |

### Cuerpo de la solicitud

Igual que `POST /api/v1/complaints`, siguiendo `ComplaintStoreRequest`. El campo `created_by` se completa automáticamente con el UUID del operador autenticado.

### Respuesta

`ComplaintResource` con relaciones básicas (`images`, `company`, `platform`).

## Ver detalles

```
GET /api/v1/complaint-book/complaints/{complaint}
```

Devuelve la reclamación con relaciones extendidas (`images`, `statusChangeEvents`, empresa completa).

## Actualizar datos

```
PUT /api/v1/complaint-book/complaints/{complaint}
```

Utiliza los mismos campos definidos en `ComplaintUpdateRequest`.

## Actualizar estado

```
PATCH /api/v1/complaint-book/complaints/{complaint}/status
```

| Campo | Tipo | Obligatorio |
| ----- | ---- | ----------- |
| `status` | string | Sí (`ComplaintStatusEnum`). |

El operador autenticado queda registrado en la línea de tiempo (`statusChangeEvents`).

## Agregar texto complementario

```
POST /api/v1/complaint-book/complaints/{complaint}/add-text
```

Sigue `TextStoreRequest` (`content`, `language`, `usage`).

## Subir imagen de evidencia

```
POST /api/v1/complaint-book/complaints/{complaint}/images
```

Usa `ImageStoreRequest`. Define `usage` (ej.: `evidence`) y proporciona la imagen por URL, Base64 o archivo.

## Eliminar una imagen específica

```
DELETE /api/v1/complaint-book/complaints/{complaint}/images/{uniqueId}
```

- Requiere permiso `destroy` / `delete`.
- Elimina la imagen cuyo identificador hexadecimal (`hex`) coincide con `uniqueId`.
- Devuelve `ComplaintResource` actualizado.

## Eliminar reclamación

```
DELETE /api/v1/complaint-book/complaints/{complaint}
```

Elimina la reclamación y devuelve `204 No Content`.

## Códigos HTTP

- 200: Operación completada con respuesta (`GET`, `PUT`, `PATCH`, `POST add-text/images`).
- 201: Imágenes creadas.
- 204: Reclamación eliminada.
- 400/422: Validación fallida.
- 401/403: Falta autenticación o habilidades.
- 404: Reclamación o imagen no encontrada.

