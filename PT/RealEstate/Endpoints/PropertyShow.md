# RealEstate – Exibir Detalhes da Propriedade

## Endpoint

```
GET /api/v1/real-estate/properties/{uuid}
```

## Autenticação

Nenhuma

## Cabeçalhos

| Cabeçalho          | Tipo     | Obrigatório | Descrição |
| ------------------ | -------- | ----------- | --------- |
| X-PUBLIC-KEY       | string   | Sim         | Chave pública da plataforma. |
| Accept-Language    | string   | Não         | Locale IETF (ex.: `pt-BR`, `en`, `es`). |

## Parâmetros

### Parâmetros de caminho

| Parâmetro | Tipo   | Obrigatório | Descrição |
| --------- | ------ | ----------- | --------- |
| uuid      | string | Sim         | Identificador único da propriedade (formato UUID) |

## Exemplos

### Exemplo de requisição (curl)

```bash
curl -X GET \
  -H "X-PUBLIC-KEY: <chave>" \
  -H "Accept-Language: pt-BR" \
  "https://sandbox.seu-dominio.com/api/v1/real-estate/properties/550e8400-e29b-41d4-a716-446655440000"
```

### Exemplo de resposta

```json
{
  "data": {
    "uuid": "550e8400-e29b-41d4-a716-446655440000",
    "title": "Apartamento Moderno de 3 Quartos",
    "description": "Lindo apartamento com vista para o mar localizado no coração de Miami. Esta propriedade deslumbrante apresenta acabamentos modernos, planta aberta e vistas deslumbrantes do Oceano Atlântico.",
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
      "street": "1234 Ocean Drive",
      "city": "Miami",
      "region": "Florida",
      "country": "US",
      "postal_code": "33139",
      "latitude": 25.7617,
      "longitude": -80.1918
    },
    "agent": {
      "uuid": "660e8400-e29b-41d4-a716-446655440001",
      "name": "John Smith",
      "email": "john.smith@agency.com",
      "phone": "+1-555-0123",
      "agency": "Premium Real Estate"
    },
    "agency": {
      "uuid": "770e8400-e29b-41d4-a716-446655440002",
      "name": "Premium Real Estate",
      "website": "https://premiumrealestate.com"
    },
    "images": [
      {
        "uuid": "880e8400-e29b-41d4-a716-446655440003",
        "url": "https://example.com/property1.jpg",
        "is_primary": true,
        "order": 1
      },
      {
        "uuid": "880e8400-e29b-41d4-a716-446655440004",
        "url": "https://example.com/property2.jpg",
        "is_primary": false,
        "order": 2
      }
    ],
    "amenities": [
      "Ar Condicionado",
      "Piscina",
      "Academia",
      "Segurança 24/7",
      "Estacionamento"
    ],
    "created_at": "2025-10-16T10:30:00Z",
    "updated_at": "2025-10-16T15:45:00Z"
  }
}
```

## Estrutura JSON Explicada

| Campo        | Tipo     | Descrição |
| ------------ | -------- | --------- |
| data | object | Detalhes da propriedade |
| data.uuid | string | Identificador único da propriedade |
| data.title | string | Título da propriedade |
| data.description | string | Descrição detalhada da propriedade |
| data.property_type | string | Tipo de propriedade (Apartamento, Casa, etc.) |
| data.rent_type | string | Tipo de transação (Venda, Aluguel, etc.) |
| data.value | numeric | Preço/valor da propriedade |
| data.currency | string | Código da moeda (ISO 4217) |
| data.size | integer | Tamanho da propriedade em metros quadrados |
| data.rooms | integer | Número total de cômodos |
| data.bedrooms | integer | Número de quartos |
| data.bathrooms | integer | Número de banheiros |
| data.garage | integer | Número de vagas de garagem |
| data.is_featured | boolean | Se a propriedade está em destaque |
| data.direct_from_owner | boolean | Se a propriedade é direto do proprietário |
| data.address | object | Informações completas do endereço |
| data.address.street | string | Endereço da rua |
| data.address.city | string | Nome da cidade |
| data.address.region | string | Nome da região/estado |
| data.address.country | string | Código do país (ISO 3166-1 alpha-2) |
| data.address.postal_code | string | Código postal/CEP |
| data.address.latitude | numeric | Latitude geográfica |
| data.address.longitude | numeric | Longitude geográfica |
| data.agent | object | Informações do agente |
| data.agent.uuid | string | Identificador único do agente |
| data.agent.name | string | Nome completo do agente |
| data.agent.email | string | Endereço de e-mail do agente |
| data.agent.phone | string | Número de telefone do agente |
| data.agent.agency | string | Nome da agência |
| data.agency | object | Informações da agência |
| data.agency.uuid | string | Identificador único da agência |
| data.agency.name | string | Nome da agência |
| data.agency.website | string | URL do site da agência |
| data.images[] | array | Imagens da propriedade |
| data.images[].uuid | string | Identificador único da imagem |
| data.images[].url | string | URL da imagem |
| data.images[].is_primary | boolean | Se a imagem é a foto principal |
| data.images[].order | integer | Ordem de exibição da imagem |
| data.amenities[] | array | Lista de comodidades da propriedade |
| data.created_at | string | Data/hora de criação da propriedade (ISO 8601) |
| data.updated_at | string | Data/hora da última atualização da propriedade (ISO 8601) |

## Status HTTP

- 200: Sucesso
- 404: Não encontrado
- 429: Limite de requests excedido
- 500: Erro interno

## Erros

```json
{
  "message": "Propriedade não encontrada"
}
```

## Notas

- A propriedade deve pertencer à plataforma especificada pelo cabeçalho `X-PUBLIC-KEY`
- Todos os campos traduzíveis respeitam o cabeçalho `Accept-Language`
- As coordenadas geográficas podem ser usadas para integração com mapas
- As imagens são retornadas ordenadas pelo campo `order`
- A imagem principal (is_primary: true) deve ser exibida como a foto principal da propriedade

## Relacionados

- [Listar Propriedades](PropertyIndex.md)
- [Listar Tipos de Propriedade](PropertyTypeIndex.md)
- [Exibir Detalhes do Agente](AgentShow.md)

## Changelog

- 2025-10-16: Documentação inicial
