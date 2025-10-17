# Bienes Raíces — Inmobiliarias: Listado

## Endpoint

```
GET /real-estate/agencies
```

## Autenticación

No es necesaria.

## Encabezados

| Encabezado      | Tipo   | Obligatorio | Descripción |
| --------------- | ------ | ----------- | ----------- |
| Accept-Language | string | No          | Localiza campos de texto cuando existan traducciones. |

## Parámetros de Consulta

| Parámetro  | Tipo   | Obligatorio | Descripción | Predeterminado |
| ---------- | ------ | ----------- | ----------- | -------------- |
| public_key | string | Sí          | Clave pública de la plataforma (`pbk`). |
| per_page   | int    | No          | Tamaño de página (1–200). | 9 |
| page       | int    | No          | Número de página. | 1 |

## Ejemplo

### Solicitud

```bash
curl -X GET "https://api.ejemplo.com/real-estate/agencies?public_key=realestate-demo&per_page=9"
```

### Respuesta (200)

```json
{
  "current_page": 1,
  "data": [
    {
      "_id": "68f252d365a4bd9f7c0321d2",
      "uuid": "363e4540-ae9d-356e-b6a3-06988311335c",
      "pbk": "88dc019b-6856-4115-a287-5f641e45c1a6",
      "name": "Fenicia Inmobiliaria",
      "email": "contacto@fenicia-inmobiliaria.com.py",
      "avatar": "https://storage.echosistema.online/main/images/real-estate/agencies/fenicia-inmobiliaria.png",
      "phones": ["+595 71 201 501", "+595 981 201 701"],
      "masked_phones": ["+595 71 201 501", "+595 981 201 701"],
      "website": "https://fenicia-inmobiliaria.com.py",
      "socials": [
        "https://www.facebook.com/fenicia-inmobiliaria",
        "https://www.instagram.com/fenicia-inmobiliaria",
        "https://www.linkedin.com/company/fenicia-inmobiliaria"
      ],
      "role": "agency",
      "properties": 11,
      "agents": 4,
      "documents": {
        "ruc": "80000001-1",
        "value": "Fenicia Inmobiliaria S.A."
      },
      "address_full": {
        "street": "Avenida Irrazábal",
        "number": "127",
        "neighborhood": "Centro",
        "city": "Encarnación",
        "state": "Itapúa",
        "country": "Paraguay"
      },
      "responsible": {
        "uuid": "06b3ad57-869b-4552-9e01-7282c57d76d3",
        "name": "Responsable Fenicia Inmobiliaria"
      },
      "created_at": "2025-10-17T14:29:39.032Z",
      "updated_at": "2025-10-17T14:29:38.968Z"
    }
  ],
  "per_page": 9,
  "total": 42
}
```

## Códigos de Estado

- 200 — Correcto
- 400 — Parámetros de paginación inválidos
- 404 — Plataforma no encontrada

## Notas

- Los resultados siempre se filtran por el `public_key` proporcionado.
- La colección se almacena en MongoDB; los campos pueden variar según el cliente.
- El campo `responsible` contiene información sobre el responsable de la inmobiliaria.
- El campo `documents` contiene documentación legal (RUC para Paraguay).
- El objeto `address_full` proporciona los detalles completos de la dirección.

## Changelog

- 2025-10-17 — Documentación inicial.
