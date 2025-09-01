# [DOMINIO] – [TÍTULO DEL ENDPOINT]

## Endpoint

```
[MÉTODO] /api/v1/[RUTA]
[OPCIONALES OTROS MÉTODOS/RUTAS]
```

## Autenticación

[Requerida – Bearer {token} con habilidad X | Ninguna]

## Encabezados

| Encabezado       | Tipo   | Requerido | Descripción |
| ---------------- | ------ | --------- | ----------- |
| Authorization    | string | Cuando aplica | Credencial `Bearer {token}`. |
| X-PUBLIC-KEY     | string | Sí        | Clave pública de la plataforma. |
| Accept-Language  | string | Cuando aplica | Locale IETF (ej.: `pt-BR`, `en`, `es`). |

## Parámetros

### Parámetros de ruta

| Parámetro | Tipo   | Requerido | Descripción |
| --------- | ------ | --------- | ----------- |
| [id]      | string | Sí        | [descripción corta] |

### Parámetros de consulta

| Parámetro | Tipo    | Requerido | Descripción | Predeterminado/Valores |
| --------- | ------- | --------- | ----------- | ---------------------- |
| [foo]     | string  | No        | [descripción] | [predeterminado] |
| [bar]     | integer | No        | [descripción] | [1-200] |

> Documente los nombres canónicos en `snake_case`. Cuando aplique, indique que se aceptan variantes (`camelCase`, `kebab-case`, `CapitalCase`).

### Cuerpo de la solicitud (cuando aplique)

```json
{
  "[campo]": "[valor]"
}
```

## Ejemplos

### Ejemplo de solicitud (curl)

```bash
curl -X [MÉTODO] \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <clave>" \
  -H "Accept-Language: es" \
  "https://sandbox.su-dominio.com/api/v1/[RUTA]?foo=bar"
```

### Ejemplo de respuesta

```json
{
  "data": [
    {
      "id": 1
    }
  ]
}
```

## Estructura JSON Explicada

| Campo       | Tipo    | Descripción |
| ----------- | ------- | ----------- |
| data[]      | array   | [lista de ítems] |
| data[].id   | integer | [identificador] |

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

Describa errores específicos (códigos/mensajes) y ejemplos de payload:

```json
{
  "message": "[mensaje]",
  "errors": {
    "campo": ["[detalle]"]
  }
}
```

## Notas

- [Reglas de negocio, predeterminados, paginación/ordenación, límites]

## Relacionados

- [Enlace relativo a endpoint relacionado](../Endpoints/OtroEndpoint.md)

## Changelog

- 2025-08-31: [descripción del cambio]

