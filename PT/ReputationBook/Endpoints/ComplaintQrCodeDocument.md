# ReputationBook – Documento com QR Code da empresa

## Endpoint

```
GET /source/company/{uuid}/qr-code-document
```

Gera uma página HTML com QR Code apontando para o perfil público da empresa.

## Autenticação

Nenhuma. A rota pertence a `routes/web.php` e pode ser embutida em PDFs ou iframes.

## Parâmetros de caminho

| Parâmetro | Tipo | Descrição |
| --------- | ---- | --------- |
| `uuid` | uuid | Identificador da empresa (`Company::uuid`). |

## Resposta

- Renderiza a view `reputationbook.complaint.qr-code-document`.
- O QR Code aponta para `https://encarnacion.defensadelconsumidor.online/companies/{uuid}/detail`.
- A URL do QR é construída via `https://api.qrserver.com/v1/create-qr-code/?size=1000x1000&data=...`.

## Status HTTP

- 200: Documento renderizado.
- 404: Empresa não encontrada.

