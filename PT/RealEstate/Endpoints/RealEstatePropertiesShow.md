# Imobiliário — Imóveis: Detalhe

Retorna o documento bruto do imóvel armazenado no MongoDB e mantém o envelope
padrão `{ data: ... }` utilizado pelos demais endpoints de RealEstate.

## Endpoint

```
GET /real-estate/properties/{property}
```

## Autenticação

Não é necessária.

## Parâmetros de Caminho

| Parâmetro | Tipo | Obrigatório | Descrição |
| --------- | ---- | ----------- | --------- |
| property  | uuid | Sim         | UUID do imóvel (binding pelo campo `uuid`). |

## Parâmetros de Consulta

| Parâmetro  | Tipo   | Obrigatório | Descrição |
| ---------- | ------ | ----------- | --------- |
| public_key | string | Não         | Opcional para garantir o escopo correto da plataforma. |

## Exemplo

### Requisição

```bash
curl -X GET "https://api.exemplo.com/real-estate/properties/e2c9c1c6-4cac-4c41-8f8e-6fd79d303001"
```

### Resposta (200)

```json
{
  "data": {
    "uuid": "e2c9c1c6-4cac-4c41-8f8e-6fd79d303001",
    "title": "Cobertura com vista para o rio",
    "city": "Lisboa",
    "region": "Lisboa",
    "country": "PT",
    "value": {
      "amount": 875000,
      "currency": "EUR"
    },
    "rentType": "sale",
    "propertyType": "penthouse",
    "rooms": 7,
    "bed": 4,
    "bath": 3,
    "garage": 2,
    "size": 215,
    "isFeatured": true,
    "directFromOwner": false,
    "createdAt": "2025-08-13T12:45:11Z",
    "details": {
      "highlights": ["Terraço panorâmico", "Vista para o rio"],
      "tags": ["luxo", "exclusivo"]
    }
  }
}
```

## Códigos de Status

- 200 — Sucesso
- 404 — Imóvel não encontrado
- 500 — Erro interno

## Notas

- O payload retorna exatamente o documento persistido no MongoDB (`properties`)
  encapsulado sob `data`.
- Envie `public_key` para evitar acesso cruzado entre plataformas diferentes.

## Changelog

- 2025-09-25 — Documentação inicial.
