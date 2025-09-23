# Usuarios de la Plataforma — Contador

- Método: `GET`
- Ruta: `/api/v1/ia/admin/users/counter`
- Autenticación: `auth:sanctum`

## Resumen
Devuelve la cantidad de usuarios por rol dentro de la plataforma actual, respetando la visibilidad mínima de roles del solicitante.

## Parámetros de consulta
- Mismos filtros que en Índice (rol/usuario/ocupación) para acotar el alcance del conteo.

## Respuesta

```json
{
  "data": {
    "counter": {
      "guest": {
        "role_id": 5,
        "name": "guest",
        "localized_name": "Guest",
        "total": 42
      }
    }
  }
}
```
