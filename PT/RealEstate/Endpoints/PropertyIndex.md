# RealEstate – Listar Propriedades

## Endpoint

```
GET /api/v1/real-estate/properties
```

## Autenticação

Nenhuma

## Cabeçalhos

| Cabeçalho          | Tipo     | Obrigatório | Descrição |
| ------------------ | -------- | ----------- | --------- |
| X-PUBLIC-KEY       | string   | Sim         | Chave pública da plataforma. |
| Accept-Language    | string   | Não         | Locale IETF (ex.: `pt-BR`, `en`, `es`). |

## Parâmetros

### Parâmetros de consulta

| Parâmetro | Tipo     | Obrigatório | Descrição | Padrão/Valores |
| --------- | -------- | ----------- | --------- | -------------- |
| uuid | array | Não | Filtrar por UUIDs de propriedades específicas | - |
| language | string | Não | Código de idioma para campos traduzíveis | - |
| city | array | Não | Filtrar por nome(s) de cidade | - |
| country | array | Não | Filtrar por código(s) de país (ISO 3166-1 alpha-2) | - |
| region | array | Não | Filtrar por nome(s) de região | - |
| rent_type | array | Não | Filtrar por tipo(s) de aluguel | - |
| property_type | array | Não | Filtrar por tipo(s) de propriedade | - |
| agency_uuid | array | Não | Filtrar por UUID(s) de agência | - |
| agency | array | Não | Filtrar por nome(s) de agência | - |
| agent_uuid | array | Não | Filtrar por UUID(s) de agente | - |
| agent | array | Não | Filtrar por nome(s) de agente | - |
| rooms | integer | Não | Filtrar por número exato de cômodos | - |
| rooms_min | integer | Não | Filtrar por número mínimo de cômodos | - |
| rooms_max | integer | Não | Filtrar por número máximo de cômodos | - |
| bed | integer | Não | Filtrar por número exato de quartos | - |
| bed_min | integer | Não | Filtrar por número mínimo de quartos | - |
| bed_max | integer | Não | Filtrar por número máximo de quartos | - |
| bath | integer | Não | Filtrar por número exato de banheiros | - |
| bath_min | integer | Não | Filtrar por número mínimo de banheiros | - |
| bath_max | integer | Não | Filtrar por número máximo de banheiros | - |
| size_min | integer | Não | Filtrar por tamanho mínimo (m²) | - |
| size_max | integer | Não | Filtrar por tamanho máximo (m²) | - |
| is_featured | boolean | Não | Filtrar propriedades em destaque | - |
| direct_from_owner | boolean | Não | Filtrar propriedades direto do proprietário | - |
| search | string | Não | Busca de texto completo nos campos da propriedade | - |
| value_currency | string | Não | Código de moeda (ISO 4217, 3 caracteres) | - |
| currency | string | Não | Parâmetro alternativo de moeda | - |
| value_min | numeric | Não | Filtrar por preço mínimo | - |
| value_max | numeric | Não | Filtrar por preço máximo | - |
| created_at | date | Não | Filtrar por data de criação | - |
| created_at_period | object | Não | Filtrar por intervalo de datas (from/to ou start/end) | - |
| sort_by | string | Não | Campo para ordenação | `createdAt` |
| direction | string | Não | Direção da ordenação | `DESC` |
| per_page | integer | Não | Itens por página | 9 (máx: 200) |
| page | integer | Não | Número da página | 1 |

> Os parâmetros aceitam variações: `snake_case`, `camelCase`, `kebab-case`, `CapitalCase`.

**Campos ordenáveis:** `created_at`, `createdat`, `created`, `createdAt`, `size`, `rooms`, `bed`, `beds`, `bedrooms`, `bath`, `baths`, `bathrooms`, `garage`, `is_featured`, `isFeatured`, `direct_from_owner`, `directFromOwner`, `value`, `price`.

## Exemplos

### Exemplo de requisição (curl)

```bash
curl -X GET \
  -H "X-PUBLIC-KEY: <chave>" \
  -H "Accept-Language: pt-BR" \
  "https://sandbox.seu-dominio.com/api/v1/real-estate/properties?per_page=9&city[]=Miami&bed_min=2&value_max=500000"
```

### Exemplo de resposta

```json
{
  "data": [
    {
      "uuid": "550e8400-e29b-41d4-a716-446655440000",
      "title": "Apartamento Moderno de 3 Quartos",
      "description": "Lindo apartamento com vista para o mar",
      "property_type": "Apartamento",
      "rent_type": "Venda",
      "value": 450000,
      "currency": "USD",
      "size": 120,
      "rooms": 3,
      "bedrooms": 3,
      "bathrooms": 2,
      "garage": 1,
      "is_featured": true,
      "direct_from_owner": false,
      "address": {
        "city": "Miami",
        "region": "Florida",
        "country": "US"
      },
      "agent": {
        "uuid": "660e8400-e29b-41d4-a716-446655440001",
        "name": "John Smith"
      },
      "images": [
        {
          "url": "https://example.com/property1.jpg",
          "is_primary": true
        }
      ],
      "created_at": "2025-10-16T10:30:00Z"
    }
  ],
  "links": {
    "first": "https://sandbox.seu-dominio.com/api/v1/real-estate/properties?page=1",
    "last": "https://sandbox.seu-dominio.com/api/v1/real-estate/properties?page=5",
    "prev": null,
    "next": "https://sandbox.seu-dominio.com/api/v1/real-estate/properties?page=2"
  },
  "meta": {
    "current_page": 1,
    "from": 1,
    "last_page": 5,
    "per_page": 9,
    "to": 9,
    "total": 42
  }
}
```

## Estrutura JSON Explicada

| Campo        | Tipo     | Descrição |
| ------------ | -------- | --------- |
| data[] | array | Lista de propriedades |
| data[].uuid | string | Identificador único da propriedade |
| data[].title | string | Título da propriedade |
| data[].description | string | Descrição da propriedade |
| data[].property_type | string | Tipo de propriedade (Apartamento, Casa, etc.) |
| data[].rent_type | string | Tipo de transação (Venda, Aluguel, etc.) |
| data[].value | numeric | Preço/valor da propriedade |
| data[].currency | string | Código da moeda |
| data[].size | integer | Tamanho da propriedade em metros quadrados |
| data[].rooms | integer | Número total de cômodos |
| data[].bedrooms | integer | Número de quartos |
| data[].bathrooms | integer | Número de banheiros |
| data[].garage | integer | Número de vagas de garagem |
| data[].is_featured | boolean | Se a propriedade está em destaque |
| data[].direct_from_owner | boolean | Se a propriedade é direto do proprietário |
| data[].address | object | Informações do endereço da propriedade |
| data[].agent | object | Informações do agente |
| data[].images[] | array | Imagens da propriedade |
| data[].created_at | string | Data/hora de criação da propriedade (ISO 8601) |
| links | object | Links de paginação |
| meta | object | Metadados de paginação |

## Status HTTP

- 200: Sucesso
- 400: Requisição inválida
- 401: Não autenticado
- 422: Erro de validação
- 429: Limite de requests excedido
- 500: Erro interno

## Erros

```json
{
  "message": "Erro de validação",
  "errors": {
    "per_page": ["O campo per page não pode ser maior que 200."],
    "country": ["O campo country deve ter 2 caracteres."]
  }
}
```

## Notas

- O parâmetro `public_key` é extraído automaticamente do cabeçalho `X-PUBLIC-KEY`
- Os resultados são paginados com padrão de 9 itens por página (máx 200)
- Parâmetros de array (city, country, etc.) podem ser passados múltiplas vezes ou como notação de array
- Filtros de data suportam vários formatos: data única ou período com from/to ou start/end
- Todos os campos traduzíveis respeitam o cabeçalho `Accept-Language`
- A ordenação padrão é por data de criação em ordem decrescente (mais recente primeiro)

## Relacionados

- [Exibir Detalhes da Propriedade](PropertyShow.md)
- [Listar Tipos de Propriedade](PropertyTypeIndex.md)
- [Listar Tipos de Aluguel](RentTypeIndex.md)
- [Listar Agentes](AgentIndex.md)

## Changelog

- 2025-10-16: Documentação inicial
