# Microservicios – Indice de Idiomas

## Endpoint

```
GET /api/v1/public/languages
```

Devuelve los idiomas mas conocidos del mundo basados en `public/storage/documents/languages.json`. Ideal para frontends publicos que necesitan poblar selectores de idioma.

---

## Autenticacion

Ninguna.

---

## Encabezados

| Encabezado      | Tipo   | Obligatorio | Descripcion |
| ---------------- | ------ | ----------- | ----------- |
| Accept-Language | string | No          | Locale IETF (`pt-BR`, `en`, `es`). |

---

## Parametros

Ninguno.

---

## Ejemplos

### Solicitud (curl)

```bash
curl -X GET "https://sandbox.ejemplo.com/api/v1/public/languages"
```

### Respuesta

```json
{
  "data": [
    {"name": "English", "code": "en"},
    {"name": "Spanish", "code": "es"},
    {"name": "Portuguese", "code": "pt"}
  ]
}
```

---

## Estructura JSON Explicada

| Campo         | Tipo   | Descripcion |
| ------------- | ------ | ----------- |
| `data[]`      | array  | Lista de idiomas soportados. |
| `data[].name` | string | Nombre del idioma. |
| `data[].code` | string | Codigo ISO-639-1. |

---

## Notas

- El endpoint lista idiomas ampliamente conocidos a nivel global.
- Agregue nuevos idiomas editando `public/storage/documents/languages.json`.
- `LanguageRepository` analiza el JSON y omite errores de lectura.

---

## Changelog

- 2025-10-03: Documentacion inicial del endpoint publico de idiomas.
