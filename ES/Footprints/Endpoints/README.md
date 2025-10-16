# Endpoints de Footprints

Este directorio contiene documentación detallada para todos los endpoints del dominio Footprints.

## Lista de Endpoints

| Endpoint | Método | Descripción |
| -------- | ------ | ----------- |
| [FootprintsIndex](./FootprintsIndex.md) | GET | Listar todas las huellas con opciones de filtrado |
| [FootprintsShow](./FootprintsShow.md) | GET | Recuperar detalles de una huella específica |
| [FootprintsStore](./FootprintsStore.md) | POST | Crear un nuevo registro de huella |

## Características Comunes

### Autenticación

La mayoría de los endpoints requieren autenticación:
- `Authorization: Bearer {token}`
- `X-PUBLIC-KEY: {clave_plataforma}`

El endpoint de creación puede usarse sin autenticación para fines de seguimiento.

### Filtrado

El endpoint de índice soporta:
- Filtrado por rango de fechas, usuario, plataforma
- Paginación con tamaño de página configurable
- Ocultación de huellas sensibles (solo masters)

## Relacionados

- [Dominio Footprints](../README.md)
