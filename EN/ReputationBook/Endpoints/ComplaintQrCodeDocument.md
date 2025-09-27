# ReputationBook – Company QR Code Document

## Endpoint

```
GET /source/company/{uuid}/qr-code-document
```

Renders an HTML page containing a QR Code that points to the company’s public profile.

## Authentication

None. The route is defined in `routes/web.php` and can be embedded in PDFs or iframes.

## Path Parameters

| Parameter | Type | Description |
| --------- | ---- | ----------- |
| `uuid` | uuid | Company identifier (`Company::uuid`). |

## Response

- Renders the `reputationbook.complaint.qr-code-document` view.
- The QR code links to `https://encarnacion.defensadelconsumidor.online/companies/{uuid}/detail`.
- The QR image is generated via `https://api.qrserver.com/v1/create-qr-code/?size=1000x1000&data=...`.

## HTTP Status Codes

- 200: Document rendered.
- 404: Company not found.

