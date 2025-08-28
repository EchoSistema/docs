# Compartido – Endpoint de Índice de Roles

## Endpoint

`GET /api/v1/roles`

Lista los roles disponibles. Los roles dependen del dominio de la plataforma solicitante. Los parámetros opcionales permiten incluir o excluir roles específicos.

---

## Autenticación

Ninguna.

---

## Request

### Parámetros de consulta

| Parámetro | Tipo | Requerido | Descripción |
| --------- | ---- | -------- | ----------- |
| `roles` | `array` | No | Incluir solo estos roles. Los valores deben ser nombres de roles válidos. |
| `except` | `array` | No | Excluir estos roles del resultado. |
| `permissions` | `boolean` | No | Cuando es `true`, incluye los permisos del rol en la respuesta. Valor predeterminado `false`. |

> Los parámetros aceptan variantes camelCase, snake_case, kebab-case o CapitalCase.

---

## Ejemplo de Respuesta

```json
{
  "data": [
    {
      "id": 1,
      "uuid": "00000000-0000-0000-0000-000000000000",
      "name": "admin",
      "title": "Administrador",
      "permissions": ["role.read", "role.write"]
    }
  ]
}
```

---

## Explicación de la Estructura JSON

| Campo | Tipo | Descripción |
| ----- | ---- | ----------- |
| `data[]` | `array` | Lista de roles. |
| `data[].id` | `integer` | Identificador del rol. |
| `data[].uuid` | `uuid` | Identificador único del rol. |
| `data[].name` | `string` | Nombre técnico del rol. |
| `data[].title` | `string` | Título localizado del rol. |
| `data[].permissions[]` | `array` | Permisos del rol cuando `permissions=true`. |

---

## Notas

* Los roles devueltos dependen del dominio de la plataforma solicitante.
* El parámetro `permissions` tiene valor predeterminado `false`.
