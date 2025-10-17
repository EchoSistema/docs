# Imobiliário — Imobiliárias: Listagem

## Endpoint

```
GET /api/v1/real-estate/agencies
```

## Autenticação

Não é necessária.

## Cabeçalhos

| Cabeçalho       | Tipo   | Obrigatório | Descrição |
| --------------- | ------ | ----------- | --------- |
| Accept-Language | string | Não         | Localiza campos textuais quando houver tradução. |

## Parâmetros de Consulta

| Parâmetro  | Tipo   | Obrigatório | Descrição | Padrão |
| ---------- | ------ | ----------- | --------- | ------ |
| public_key | string | Sim         | Chave pública da plataforma (`pbk`). |
| per_page   | int    | Não         | Tamanho da página (1–200). | 9 |
| page       | int    | Não         | Página desejada. | 1 |

## Exemplo

### Requisição

```bash
curl -X GET "https://sandbox.echosistema.online/api/v1/real-estate/agencies?public_key=1396fd3b-d56b-362f-8ebb-c068bb910b69&per_page=9&page=1"
```

### Resposta (200)

```json
{
  "current_page": 1,
  "data": [
    {
      "avatar": "https://storage.echosistema.online/main/images/real-estate/agencies/fenicia-inmobiliaria.png",
      "name": "Fenicia Inmobiliaria",
      "email": "contacto@fenicia-inmobiliaria.com.py",
      "phones": [
        "+595 71 201 501",
        "+595 981 201 701"
      ],
      "masked_phones": [
        "+595 71 201 501",
        "+595 981 201 701"
      ],
      "socials": [
        "https://www.facebook.com/fenicia-inmobiliaria",
        "https://www.instagram.com/fenicia-inmobiliaria",
        "https://www.linkedin.com/company/fenicia-inmobiliaria"
      ],
      "website": "https://fenicia-inmobiliaria.com.py",
      "properties": 11,
      "agents": 4,
      "role": "agency",
      "documents": {
        "ruc": "80000001-1",
        "value": "Fenicia Inmobiliaria S.A."
      },
      "address_full": {
        "street": "Avenida Irrazábal",
        "number": "127",
        "neighborhood": "Centro",
        "state": "Itapúa",
        "city": "Encarnación",
        "country": "Paraguay",
        "zip": "6001"
      },
      "responsible": {
        "uuid": "685e9735-c0a8-45b2-90ba-f6324cf0f218",
        "name": "Responsable Fenicia Inmobiliaria"
      },
      "pbk": "1396fd3b-d56b-362f-8ebb-c068bb910b69",
      "updated_at": "2025-10-17T16:49:46.409000Z",
      "uuid": "363e4540-ae9d-356e-b6a3-06988311335c",
      "created_at": "2025-10-17T16:49:46.390000Z",
      "full_address": "Avenida Irrazábal, 127, Centro, Itapúa, Encarnación, Paraguay, 6001",
      "id": "68f273aa929b4c71aa0b9ab5"
    },
    {
      "avatar": "https://storage.echosistema.online/main/images/real-estate/agencies/encasa-propiedades.png",
      "name": "Encasa Propiedades",
      "email": "contacto@encasa-propiedades.com.py",
      "phones": [
        "+595 71 202 502",
        "+595 981 202 702"
      ],
      "masked_phones": [
        "+595 71 202 502",
        "+595 981 202 702"
      ],
      "socials": [
        "https://www.facebook.com/encasa-propiedades",
        "https://www.instagram.com/encasa-propiedades",
        "https://www.linkedin.com/company/encasa-propiedades"
      ],
      "website": "https://encasa-propiedades.com.py",
      "properties": 12,
      "agents": 5,
      "role": "agency",
      "documents": {
        "ruc": "80000002-2",
        "value": "Encasa Propiedades S.A."
      },
      "address_full": {
        "street": "Avenida Irrazábal",
        "number": "134",
        "neighborhood": "Centro",
        "state": "Itapúa",
        "city": "Encarnación",
        "country": "Paraguay",
        "zip": "6002"
      },
      "responsible": {
        "uuid": "98a5c508-2828-46e8-9599-32837e9abeb2",
        "name": "Responsable Encasa Propiedades"
      },
      "pbk": "1396fd3b-d56b-362f-8ebb-c068bb910b69",
      "updated_at": "2025-10-17T16:49:46.418000Z",
      "uuid": "66c9ebdd-4537-356e-bb13-f0df619b36ae",
      "created_at": "2025-10-17T16:49:46.415000Z",
      "full_address": "Avenida Irrazábal, 134, Centro, Itapúa, Encarnación, Paraguay, 6002",
      "id": "68f273aa929b4c71aa0b9ab6"
    },
    {
      "avatar": "https://storage.echosistema.online/main/images/real-estate/agencies/ita-realty.png",
      "name": "Itá Realty",
      "email": "contacto@ita-realty.com.py",
      "phones": [
        "+595 71 203 503",
        "+595 981 203 703"
      ],
      "masked_phones": [
        "+595 71 203 503",
        "+595 981 203 703"
      ],
      "socials": [
        "https://www.facebook.com/ita-realty",
        "https://www.instagram.com/ita-realty",
        "https://www.linkedin.com/company/ita-realty"
      ],
      "website": "https://ita-realty.com.py",
      "properties": 13,
      "agents": 6,
      "role": "agency",
      "documents": {
        "ruc": "80000003-3",
        "value": "Itá Realty S.A."
      },
      "address_full": {
        "street": "Avenida Irrazábal",
        "number": "141",
        "neighborhood": "Centro",
        "state": "Central",
        "city": "San Lorenzo",
        "country": "Paraguay",
        "zip": "6003"
      },
      "responsible": {
        "uuid": "2dd38c9b-4b15-4a47-abb9-d824235265e6",
        "name": "Responsable Itá Realty"
      },
      "pbk": "1396fd3b-d56b-362f-8ebb-c068bb910b69",
      "updated_at": "2025-10-17T16:49:46.424000Z",
      "uuid": "d48734c9-82d7-3b39-91d8-fa72991e6c4c",
      "created_at": "2025-10-17T16:49:46.421000Z",
      "full_address": "Avenida Irrazábal, 141, Centro, Central, San Lorenzo, Paraguay, 6003",
      "id": "68f273aa929b4c71aa0b9ab7"
    },
    {
      "avatar": "https://storage.echosistema.online/main/images/real-estate/agencies/horizonte-inmobiliaria.png",
      "name": "Horizonte Inmobiliaria",
      "email": "contacto@horizonte-inmobiliaria.com.py",
      "phones": [
        "+595 71 204 504",
        "+595 981 204 704"
      ],
      "masked_phones": [
        "+595 71 204 504",
        "+595 981 204 704"
      ],
      "socials": [
        "https://www.facebook.com/horizonte-inmobiliaria",
        "https://www.instagram.com/horizonte-inmobiliaria",
        "https://www.linkedin.com/company/horizonte-inmobiliaria"
      ],
      "website": "https://horizonte-inmobiliaria.com.py",
      "properties": 14,
      "agents": 7,
      "role": "agency",
      "documents": {
        "ruc": "80000004-4",
        "value": "Horizonte Inmobiliaria S.A."
      },
      "address_full": {
        "street": "Avenida Irrazábal",
        "number": "148",
        "neighborhood": "Centro",
        "state": "Central",
        "city": "Asunción",
        "country": "Paraguay",
        "zip": "6004"
      },
      "responsible": {
        "uuid": "41d3214d-6502-45fb-9681-c181393b9bb5",
        "name": "Responsable Horizonte Inmobiliaria"
      },
      "pbk": "1396fd3b-d56b-362f-8ebb-c068bb910b69",
      "updated_at": "2025-10-17T16:49:46.428000Z",
      "uuid": "d7963ebf-2c3d-32d7-9263-e99f0525db3b",
      "created_at": "2025-10-17T16:49:46.426000Z",
      "full_address": "Avenida Irrazábal, 148, Centro, Central, Asunción, Paraguay, 6004",
      "id": "68f273aa929b4c71aa0b9ab8"
    },
    {
      "avatar": "https://storage.echosistema.online/main/images/real-estate/agencies/terrasul-paraguay.png",
      "name": "TerraSul Paraguay",
      "email": "contacto@terrasul-paraguay.com.py",
      "phones": [
        "+595 71 205 505",
        "+595 981 205 705"
      ],
      "masked_phones": [
        "+595 71 205 505",
        "+595 981 205 705"
      ],
      "socials": [
        "https://www.facebook.com/terrasul-paraguay",
        "https://www.instagram.com/terrasul-paraguay",
        "https://www.linkedin.com/company/terrasul-paraguay"
      ],
      "website": "https://terrasul-paraguay.com.py",
      "properties": 15,
      "agents": 8,
      "role": "agency",
      "documents": {
        "ruc": "80000005-5",
        "value": "TerraSul Paraguay S.A."
      },
      "address_full": {
        "street": "Avenida Irrazábal",
        "number": "155",
        "neighborhood": "Centro",
        "state": "Central",
        "city": "Lambaré",
        "country": "Paraguay",
        "zip": "6005"
      },
      "responsible": {
        "uuid": "56f2a94d-eb48-4238-af61-388c9df55105",
        "name": "Responsable TerraSul Paraguay"
      },
      "pbk": "1396fd3b-d56b-362f-8ebb-c068bb910b69",
      "updated_at": "2025-10-17T16:49:46.432000Z",
      "uuid": "1948127e-d774-39c3-9446-e3d2ccfdfe8f",
      "created_at": "2025-10-17T16:49:46.430000Z",
      "full_address": "Avenida Irrazábal, 155, Centro, Central, Lambaré, Paraguay, 6005",
      "id": "68f273aa929b4c71aa0b9ab9"
    },
    {
      "avatar": "https://storage.echosistema.online/main/images/real-estate/agencies/alquila-online.png",
      "name": "Alquila Online",
      "email": "contacto@alquila-online.com.py",
      "phones": [
        "+595 71 206 506",
        "+595 981 206 706"
      ],
      "masked_phones": [
        "+595 71 206 506",
        "+595 981 206 706"
      ],
      "socials": [
        "https://www.facebook.com/alquila-online",
        "https://www.instagram.com/alquila-online",
        "https://www.linkedin.com/company/alquila-online"
      ],
      "website": "https://alquila-online.com.py",
      "properties": 16,
      "agents": 9,
      "role": "agency",
      "documents": {
        "ruc": "80000006-6",
        "value": "Alquila Online S.A."
      },
      "address_full": {
        "street": "Avenida Irrazábal",
        "number": "162",
        "neighborhood": "Centro",
        "state": "Itapúa",
        "city": "Encarnación",
        "country": "Paraguay",
        "zip": "6006"
      },
      "responsible": {
        "uuid": "4859d561-805f-485b-a26c-252f4b07271c",
        "name": "Responsable Alquila Online"
      },
      "pbk": "1396fd3b-d56b-362f-8ebb-c068bb910b69",
      "updated_at": "2025-10-17T16:49:46.437000Z",
      "uuid": "ee78742d-d43f-381a-a81e-a20f244f5f10",
      "created_at": "2025-10-17T16:49:46.435000Z",
      "full_address": "Avenida Irrazábal, 162, Centro, Itapúa, Encarnación, Paraguay, 6006",
      "id": "68f273aa929b4c71aa0b9aba"
    },
    {
      "avatar": "https://storage.echosistema.online/main/images/real-estate/agencies/vivaparaguay-realty.png",
      "name": "VivaParaguay Realty",
      "email": "contacto@vivaparaguay-realty.com.py",
      "phones": [
        "+595 71 207 507",
        "+595 981 207 707"
      ],
      "masked_phones": [
        "+595 71 207 507",
        "+595 981 207 707"
      ],
      "socials": [
        "https://www.facebook.com/vivaparaguay-realty",
        "https://www.instagram.com/vivaparaguay-realty",
        "https://www.linkedin.com/company/vivaparaguay-realty"
      ],
      "website": "https://vivaparaguay-realty.com.py",
      "properties": 17,
      "agents": 3,
      "role": "agency",
      "documents": {
        "ruc": "80000007-7",
        "value": "VivaParaguay Realty S.A."
      },
      "address_full": {
        "street": "Avenida Irrazábal",
        "number": "169",
        "neighborhood": "Centro",
        "state": "Alto Paraná",
        "city": "Ciudad del Este",
        "country": "Paraguay",
        "zip": "6007"
      },
      "responsible": {
        "uuid": "750c8bf1-3a11-4f0b-af45-4a535f0aaf71",
        "name": "Responsable VivaParaguay Realty"
      },
      "pbk": "1396fd3b-d56b-362f-8ebb-c068bb910b69",
      "updated_at": "2025-10-17T16:49:46.441000Z",
      "uuid": "25738013-1730-3c3c-9230-9e6090c713ae",
      "created_at": "2025-10-17T16:49:46.439000Z",
      "full_address": "Avenida Irrazábal, 169, Centro, Alto Paraná, Ciudad del Este, Paraguay, 6007",
      "id": "68f273aa929b4c71aa0b9abb"
    },
    {
      "avatar": "https://storage.echosistema.online/main/images/real-estate/agencies/guara-homes.png",
      "name": "Guara Homes",
      "email": "contacto@guara-homes.com.py",
      "phones": [
        "+595 71 208 508",
        "+595 981 208 708"
      ],
      "masked_phones": [
        "+595 71 208 508",
        "+595 981 208 708"
      ],
      "socials": [
        "https://www.facebook.com/guara-homes",
        "https://www.instagram.com/guara-homes",
        "https://www.linkedin.com/company/guara-homes"
      ],
      "website": "https://guara-homes.com.py",
      "properties": 18,
      "agents": 4,
      "role": "agency",
      "documents": {
        "ruc": "80000008-8",
        "value": "Guara Homes S.A."
      },
      "address_full": {
        "street": "Avenida Irrazábal",
        "number": "176",
        "neighborhood": "Centro",
        "state": "Itapúa",
        "city": "Encarnación",
        "country": "Paraguay",
        "zip": "6008"
      },
      "responsible": {
        "uuid": "8628bf1e-3f10-4aa7-924b-11d6d49c997b",
        "name": "Responsable Guara Homes"
      },
      "pbk": "1396fd3b-d56b-362f-8ebb-c068bb910b69",
      "updated_at": "2025-10-17T16:49:46.446000Z",
      "uuid": "13b6b45f-1dd1-3249-a89a-069db34c9f24",
      "created_at": "2025-10-17T16:49:46.444000Z",
      "full_address": "Avenida Irrazábal, 176, Centro, Itapúa, Encarnación, Paraguay, 6008",
      "id": "68f273aa929b4c71aa0b9abc"
    },
    {
      "avatar": "https://storage.echosistema.online/main/images/real-estate/agencies/costa-norte-propiedades.png",
      "name": "Costa Norte Propiedades",
      "email": "contacto@costa-norte-propiedades.com.py",
      "phones": [
        "+595 71 209 509",
        "+595 981 209 709"
      ],
      "masked_phones": [
        "+595 71 209 509",
        "+595 981 209 709"
      ],
      "socials": [
        "https://www.facebook.com/costa-norte-propiedades",
        "https://www.instagram.com/costa-norte-propiedades",
        "https://www.linkedin.com/company/costa-norte-propiedades"
      ],
      "website": "https://costa-norte-propiedades.com.py",
      "properties": 19,
      "agents": 5,
      "role": "agency",
      "documents": {
        "ruc": "80000009-0",
        "value": "Costa Norte Propiedades S.A."
      },
      "address_full": {
        "street": "Avenida Irrazábal",
        "number": "183",
        "neighborhood": "Centro",
        "state": "Ñeembucú",
        "city": "Pilar",
        "country": "Paraguay",
        "zip": "6009"
      },
      "responsible": {
        "uuid": "0c74bf54-0519-479c-ab51-fb9ab26c2414",
        "name": "Responsable Costa Norte Propiedades"
      },
      "pbk": "1396fd3b-d56b-362f-8ebb-c068bb910b69",
      "updated_at": "2025-10-17T16:49:46.450000Z",
      "uuid": "589baab9-e760-39f9-a3d4-da1683b8d1cc",
      "created_at": "2025-10-17T16:49:46.449000Z",
      "full_address": "Avenida Irrazábal, 183, Centro, Ñeembucú, Pilar, Paraguay, 6009",
      "id": "68f273aa929b4c71aa0b9abd"
    }
  ],
  "first_page_url": "https://sandbox.echosistema.online/api/v1/real-estate/agencies?page=1",
  "from": 1,
  "last_page": 3,
  "last_page_url": "https://sandbox.echosistema.online/api/v1/real-estate/agencies?page=3",
  "links": [
    {
      "url": null,
      "label": "&laquo; Previous",
      "active": false
    },
    {
      "url": "https://sandbox.echosistema.online/api/v1/real-estate/agencies?page=1",
      "label": "1",
      "active": true
    },
    {
      "url": "https://sandbox.echosistema.online/api/v1/real-estate/agencies?page=2",
      "label": "2",
      "active": false
    },
    {
      "url": "https://sandbox.echosistema.online/api/v1/real-estate/agencies?page=3",
      "label": "3",
      "active": false
    },
    {
      "url": "https://sandbox.echosistema.online/api/v1/real-estate/agencies?page=2",
      "label": "Next &raquo;",
      "active": false
    }
  ],
  "next_page_url": "https://sandbox.echosistema.online/api/v1/real-estate/agencies?page=2",
  "path": "https://sandbox.echosistema.online/api/v1/real-estate/agencies",
  "per_page": 9,
  "prev_page_url": null,
  "to": 9,
  "total": 19
}
```

## Códigos de Status

- 200 — Sucesso
- 400 — Parâmetros de paginação inválidos
- 404 — Plataforma não encontrada

## Notas

- Todos os resultados pertencem ao `public_key` informado.
- Os campos são provenientes do MongoDB e podem variar conforme o tenant.
- O campo `responsible` contém informações sobre o responsável pela imobiliária.
- O campo `documents` contém documentação legal (RUC para o Paraguai).
- O objeto `address_full` fornece os detalhes completos do endereço e o campo `full_address` traz a representação concatenada.
- O campo `id` é o identificador MongoDB (string) da agência.

## Changelog

- 2025-10-17 — Exemplo e metadados de paginação atualizados para o retorno real.
- 2025-10-17 — Documentação inicial.
