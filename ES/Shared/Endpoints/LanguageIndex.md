# Compartido – Indice de Idiomas Disponibles

## Endpoint

`GET /api/v1/languages`

Devuelve los idiomas soportados por el backoffice segun `LanguageEnum`, incluyendo el codigo y el nombre mostrado (`native_name`).

---

## Autenticacion

Ninguna.

---

## Request

Sin parametros.

---

## Ejemplo de Respuesta

```json
{
  "data": [
    {
      "name": "PT_BR",
      "code": "pt-BR",
      "native_name": "Portugues (Brasil)"
    },
    {
      "name": "EN",
      "code": "en",
      "native_name": "English"
    }
  ]
}
```

---

## Estructura JSON Explicada

| Campo | Tipo | Descripcion |
| ----- | ---- | ----------- |
| `data[]` | `array` | Lista de idiomas soportados. |
| `data[].name` | `string` | Nombre del enum (`PT_BR`, `EN`, `ES`, `GN`). |
| `data[].code` | `string` | Codigo ISO del idioma. |
| `data[].native_name` | `string` | Nombre mostrado del idioma. |

---

## Notas

* El orden sigue la declaracion en `LanguageEnum`.

---

## Changelog

- 2025-10-03: Documentacion inicial del endpoint `/api/v1/languages`.
