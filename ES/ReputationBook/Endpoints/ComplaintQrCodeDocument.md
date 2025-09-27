# ReputationBook – Documento con código QR de la empresa

## Endpoint

```
GET /source/company/{uuid}/qr-code-document
```

Genera una página HTML con un código QR que apunta al perfil público de la empresa.

## Autenticación

Ninguna. La ruta está definida en `routes/web.php` y puede incrustarse en PDF o iframes.

## Parámetros de ruta

| Parámetro | Tipo | Descripción |
| --------- | ---- | ----------- |
| `uuid` | uuid | Identificador de la empresa (`Company::uuid`). |

## Respuesta

- Renderiza la vista `reputationbook.complaint.qr-code-document`.
- El código QR apunta a `https://encarnacion.defensadelconsumidor.online/companies/{uuid}/detail`.
- La imagen del QR se genera mediante `https://api.qrserver.com/v1/create-qr-code/?size=1000x1000&data=...`.

## Códigos HTTP

- 200: Documento renderizado.
- 404: Empresa no encontrada.

