# Imobiliário — Imóveis: Listagem

## Endpoint

```
GET /real-estate/properties
```

## Autenticação

Não é necessária. Informe o `public_key` para limitar a listagem à plataforma certa.

## Cabeçalhos

| Cabeçalho       | Tipo   | Obrigatório | Descrição |
| --------------- | ------ | ----------- | --------- |
| Accept-Language | string | Não         | Locale usado para localizar campos textuais (`pt-BR`, `en`, `es`, …). |

## Parâmetros de Consulta

### Paginação e Ordenação

| Parâmetro | Tipo   | Obrigatório | Descrição | Padrão |
| --------- | ------ | ----------- | --------- | ------ |
| page      | int    | Não         | Página desejada. | 1 |
| per_page  | int    | Não         | Tamanho da página (1–200). | 9 |
| sort_by   | string | Não         | Campo para ordenação. Aceita `created_at`, `size`, `rooms`, `bed`, `bath`, `garage`, `is_featured`, `direct_from_owner`, `value`. | `createdAt` |
| direction | string | Não         | `asc` ou `desc` (case insensitive). | `DESC` |

### Identificadores e Propriedade

| Parâmetro     | Tipo   | Obrigatório | Descrição |
| ------------- | ------ | ----------- | --------- |
| public_key[]  | string | Não         | Filtra pelo(s) `pbk` da plataforma. |
| uuid[]        | uuid   | Não         | Filtra por UUID(s) do imóvel. |
| agency_uuid[] | uuid   | Não         | Filtra por UUID(s) da imobiliária. |
| agent_uuid[]  | uuid   | Não         | Filtra por UUID(s) do corretor. |

### Localização e Tipologia

| Parâmetro       | Tipo   | Obrigatório | Descrição |
| --------------- | ------ | ----------- | --------- |
| city[]          | string | Não         | Filtra por cidade. |
| region[]        | string | Não         | Filtra por estado/região. |
| country[]       | string | Não         | Filtra por país (código ISO). |
| rent_type[]     | string | Não         | Filtra por tipo de negociação. |
| property_type[] | string | Não         | Filtra por tipo de imóvel. |

### Capacidade e Comodidades

| Parâmetro | Tipo | Obrigatório | Descrição |
| --------- | ---- | ----------- | --------- |
| rooms / rooms_min / rooms_max | int | Não | Filtro por número de cômodos. |
| bed / bed_min / bed_max       | int | Não | Filtro por dormitórios. |
| bath / bath_min / bath_max    | int | Não | Filtro por banheiros. |
| size_min / size_max           | int | Não | Faixa de metragem. |
| garage                        | int | Não | Número exato de vagas. |

### Destaques e Valor

| Parâmetro         | Tipo    | Obrigatório | Descrição |
| ----------------- | ------- | ----------- | --------- |
| is_featured       | boolean | Não         | Apenas imóveis destacados. |
| direct_from_owner | boolean | Não         | Apenas imóveis direto com o proprietário. |
| search            | string  | Não         | Busca textual em campos diversos. |
| value_currency    | string  | Não         | Código da moeda (`BRL`, `USD`, …). |
| value_min/max     | number  | Não         | Faixa de valor. |

### Datas

| Parâmetro               | Tipo | Obrigatório | Descrição |
| ----------------------- | ---- | ----------- | --------- |
| created_at              | date | Não         | Data exata de criação. |
| created_at_period[from] | date | Não         | Data inicial (aliases: `start`, `0`). |
| created_at_period[to]   | date | Não         | Data final (aliases: `end`, `1`). |

> Filtros em formato array aceitam parâmetros repetidos (`?uuid[]=a&uuid[]=b`) ou notação com índices (`uuid[0]=a`).

## Exemplo

### Requisição

```bash
curl -X GET \
  "https://api.exemplo.com/real-estate/properties?public_key=realestate-demo&city=Lisboa&per_page=9"
```

### Resposta (200)

```json
{
  "current_page": 1,
  "data": [
    {
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
      "createdAt": "2025-08-13T12:45:11Z"
    }
  ],
  "first_page_url": "https://api.exemplo.com/real-estate/properties?page=1",
  "from": 1,
  "last_page": 12,
  "last_page_url": "https://api.exemplo.com/real-estate/properties?page=12",
  "links": [],
  "next_page_url": "https://api.exemplo.com/real-estate/properties?page=2",
  "path": "https://api.exemplo.com/real-estate/properties",
  "per_page": 9,
  "prev_page_url": null,
  "to": 9,
  "total": 108
}
```

## Códigos de Status

- 200 — Sucesso
- 400 — Erro de validação nos filtros
- 404 — Plataforma não encontrada
- 500 — Erro interno inesperado

## Notas

- Quando informado, o `public_key` limita automaticamente os resultados.
- Alias em camelCase ou snake_case são aceitos no `sort_by`.
- A resposta segue o formato padrão do `LengthAwarePaginator` do Laravel.

## Changelog

- 2025-09-25 — Documentação inicial.
