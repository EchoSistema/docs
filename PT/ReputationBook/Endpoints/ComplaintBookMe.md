# ReputationBook – Plataforma vinculada à empresa

## Endpoint

```
GET /api/v1/complaint-book/me
```

> Esta rota pertence ao grupo backoffice (`ability: backoffice,domain:reputationbook`). Apesar de compartilhar o caminho com o endpoint público do usuário, a autorização garante que apenas colaboradores da plataforma acessem esta versão.

## Autenticação

Obrigatória – `Bearer {token}` com habilidades `backoffice` e `domain:reputationbook`.

## Parâmetros de consulta

| Parâmetro | Tipo | Obrigatório | Descrição |
| --------- | ---- | ----------- | --------- |
| `platform_id` | integer | Sim | ID interno da plataforma. |
| `rel` | string | Não | Lista separada por vírgula de relações extras para carregar (`addresses`, `contacts`, `documents`, ...). Valores são camelizados pelo controller. |

## Exemplo de resposta

```json
{
  "data": {
    "uuid": "8d0f47bf-5e40-4bb4-9dc1-1ae75da3f148",
    "name": "Defesa do Consumidor",
    "slug": "defesa-consumidor",
    "is_governamental": true,
    "platform": {
      "email": "contato@plataforma.gov",
      "favicon": "https://cdn.assets/favicon.ico",
      "logo": {
        "main": "https://cdn.assets/logo.png",
        "square": "https://cdn.assets/logo-square.png",
        "rounded": "https://cdn.assets/logo-rounded.png",
        "rectangular": "https://cdn.assets/logo-rect.png"
      },
      "domain_area": {
        "uuid": "963e42f1-5c2e-4a86-8866-2e05fcf7f3f9",
        "name": "Defesa do Consumidor"
      }
    },
    "addresses": [
      {
        "type": "headquarters",
        "zipcode": "70000-000",
        "address_one": "Esplanada, bloco A",
        "city": { "id": 1, "name": "Brasília" },
        "state": { "id": 7, "name": "Distrito Federal" },
        "country": { "id": 33, "name": "Brasil" }
      }
    ],
    "contacts": [
      {
        "id": 10,
        "type": "telephone",
        "country_code": "+55",
        "number": "6130000000",
        "full_number": "+556130000000"
      }
    ],
    "documents": [
      {
        "uuid": "b3d2e698-4a6e-41d3-9a50-288e7b2c506b",
        "type": "ruc",
        "value": "80012345-6",
        "expires_at": null
      }
    ],
    "coverages": [
      {
        "type": "city",
        "values": ["Encarnación", "Posadas"]
      }
    ]
  }
}
```

## Status HTTP

- 200: Empresa vinculada retornada.
- 400/422: Parâmetros ausentes (ex.: `platform_id`).
- 404: Plataforma não possui empresa associada.

