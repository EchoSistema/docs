# Bienes Raíces — Tipos de Propiedad para Alquiler: Listado

## Endpoint

```
GET /real-estate/rental-properties/types
```

## Autenticación

No es necesaria.

## Encabezados

| Encabezado      | Tipo   | Obligatorio | Descripción |
| --------------- | ------ | ----------- | ----------- |
| Accept-Language | string | No          | Ayuda a devolver los títulos en el idioma deseado. |

## Parámetros de Consulta

| Parámetro | Tipo   | Obligatorio | Descripción |
| --------- | ------ | ----------- | ----------- |
| language  | string | No          | Idioma para obtener los títulos (usa el valor por defecto si falta). |

## Ejemplo

### Solicitud

```bash
curl -X GET "https://api.ejemplo.com/real-estate/rental-properties/types?language=es"
```

### Respuesta (200)

```json
{
  "data": [
    {
      "uuid": "c1927cd4-40c1-4d92-ab2e-4c05bb560001",
      "slug": "apartment",
      "name": "Apartamento"
    },
    {
      "uuid": "f0278cbe-2665-42c3-94c3-05c4371c0002",
      "slug": "loft",
      "name": "Loft"
    }
  ],
  "meta": {
    "domainArea": {
      "uuid": "df742b1d-1be8-4c73-9c5f-3c7076000101",
      "name": "Bienes Raíces"
    }
  }
}
```

## Códigos de Estado

- 200 — Correcto
- 404 — Área de dominio `realestate` no configurada

## Notas

- El endpoint carga el área de dominio `realestate` y devuelve sus tipos asociados respetando el idioma solicitado o el valor por defecto.
- Si no encuentra el área, devuelve 404 con el mensaje traducido `common.not_found`.

## Changelog

- 2025-09-25 — Documentación inicial.
