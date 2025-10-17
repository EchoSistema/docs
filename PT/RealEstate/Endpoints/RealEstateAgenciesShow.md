# Imobiliário — Imobiliárias: Detalhes

## Endpoint

```
GET /real-estate/agencies/{agency}
```

## Autenticação

Não é necessária.

## Parâmetros de Rota

| Parâmetro | Tipo | Obrigatório | Descrição |
| --------- | ---- | ----------- | --------- |
| agency    | uuid | Sim         | UUID da imobiliária (`agencies.uuid`). |

## Exemplo

### Requisição

```bash
curl -X GET "https://api.exemplo.com/real-estate/agencies/363e4540-ae9d-356e-b6a3-06988311335c"
```

### Resposta (200)

```json
{
  "data": {
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
}
```

## Códigos de Status

- 200 — Sucesso
- 404 — Imobiliária não encontrada

## Notas

- O documento é recuperado do MongoDB (`agencies`).
- Não é necessário fornecer a chave da plataforma porque o UUID é único por tenant.
- O campo `responsible` contém informações sobre o responsável pela imobiliária.
- O campo `documents` contém documentação legal (RUC para o Paraguai).
- O objeto `address_full` fornece os detalhes completos do endereço.
- O campo `properties` indica o número de propriedades gerenciadas pela imobiliária.
- O campo `agents` indica o número de corretores que trabalham para a imobiliária.

## Changelog

- 2025-10-17 — Documentação inicial.
