# Imobiliário — Imóveis para Locação: Listagem

## Endpoint

```
GET /real-estate/rental-properties
```

## Autenticação

Não exige token; basta o contexto da plataforma.

## Cabeçalhos

| Cabeçalho       | Tipo   | Obrigatório | Descrição |
| --------------- | ------ | ----------- | --------- |
| Accept-Language | string | Não         | Define o idioma preferido para títulos e descrições. |

## Parâmetros de Consulta

### Paginação e Alternância

| Parâmetro   | Tipo    | Obrigatório | Descrição | Padrão |
| ----------- | ------- | ----------- | --------- | ------ |
| per_page    | int     | Não         | Itens por página quando paginado. | 15 |
| page        | int     | Não         | Número da página. | 1 |
| no_paginate | boolean | Não         | Quando `true`, retorna toda a coleção sem paginação. | false |

### Localização e Filtros Gerais

| Parâmetro        | Tipo   | Descrição |
| ---------------- | ------ | --------- |
| language         | string | Idioma para resolver traduções (`pt-BR`, `en`, ...). |
| payment_interval | string | Intervalo de pagamento (enum: `daily`, `weekly`, `monthly`, ...). |
| is_available     | boolean| Flag de disponibilidade (forçado para `true` pelo endpoint). |
| uuid             | uuid   | Filtra por UUID do imóvel. |
| user_uuid        | uuid   | Filtra pelo criador. |
| type             | string | Filtra por slug do tipo associado. |

### Endereço

| Parâmetro | Tipo  | Descrição |
| --------- | ----- | --------- |
| zipcode   | string| CEP. |
| city / city_id / city_uuid / city_name | mixed | Aceita IDs numéricos, UUID ou nome da cidade. |
| state / state_id / state_uuid / state_name | mixed | Mesma lógica para estado. |
| country / country_id / country_uuid / country_code / country_name | mixed | Busca por país (ID, UUID ou código ISO). |

### Texto

| Parâmetro   | Tipo   | Descrição |
| ----------- | ------ | --------- |
| title       | string | Filtra pelo título traduzido. |
| description | string | Filtra pelo texto/descrição. |

## Exemplo

### Requisição

```bash
curl -X GET \
  "https://api.exemplo.com/real-estate/rental-properties?language=pt-BR&per_page=12"
```

### Resposta (200)

```json
{
  "data": [
    {
      "uuid": "c51823c1-8d41-477e-a3b7-7108faae0001",
      "payment_interval": "monthly",
      "type": {
        "uuid": "b7174d07-7e28-4a9b-8f9a-36f0f5270101",
        "name": "Apartamento mobiliado"
      },
      "title": "Estúdio próximo ao metrô",
      "renter": "Skyline Rentals",
      "renter_type": "company",
      "image": {
        "usage": "card",
        "url": "https://cdn.exemplo.com/rental-properties/c51823c1/card.jpg",
        "name": "cover"
      },
      "address": "Av. Paulista, 1000 — Bela Vista, São Paulo/SP",
      "is_available": true
    }
  ],
  "links": {
    "first": "https://api.exemplo.com/real-estate/rental-properties?page=1",
    "last": "https://api.exemplo.com/real-estate/rental-properties?page=8",
    "prev": null,
    "next": "https://api.exemplo.com/real-estate/rental-properties?page=2"
  },
  "meta": {
    "current_page": 1,
    "from": 1,
    "last_page": 8,
    "path": "https://api.exemplo.com/real-estate/rental-properties",
    "per_page": 12,
    "to": 12,
    "total": 92
  }
}
```

## Códigos de Status

- 200 — Sucesso
- 400 — Filtros inválidos
- 403 — Imóvel não pertence à plataforma ativa
- 500 — Erro interno

## Notas

- O endpoint sempre força `is_available = true`.
- Títulos, descrições e nomes de tipos respeitam a query `language` com fallback para o padrão.
- Quando `no_paginate=true`, os blocos `links` e `meta` são omitidos, mas os itens continuam dentro de `data`.

## Changelog

- 2025-09-25 — Documentação inicial.
