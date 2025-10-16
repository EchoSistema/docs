# RealEstate – Exibir Detalhes do Agente

## Endpoint

```
GET /api/v1/real-estate/agents/{uuid}
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
| uuid      | string | Sim         | Identificador único do agente (formato UUID) |

## Exemplos

### Exemplo de requisição (curl)

```bash
curl -X GET \
  -H "X-PUBLIC-KEY: <chave>" \
  -H "Accept-Language: pt-BR" \
  "https://sandbox.seu-dominio.com/api/v1/real-estate/agents/660e8400-e29b-41d4-a716-446655440001"
```

### Exemplo de resposta

```json
{
  "data": {
    "uuid": "660e8400-e29b-41d4-a716-446655440001",
    "name": "John Smith",
    "email": "john.smith@agency.com",
    "phone": "+1-555-0123",
    "mobile": "+1-555-0124",
    "bio": "Agente imobiliário experiente com mais de 10 anos no mercado de Miami. Especializado em propriedades de luxo e casas à beira-mar.",
    "agency": {
      "uuid": "770e8400-e29b-41d4-a716-446655440002",
      "name": "Premium Real Estate",
      "address": "123 Main Street, Miami, FL",
      "website": "https://premiumrealestate.com"
    },
    "avatar": "https://example.com/agents/john-smith.jpg",
    "properties_count": 25,
    "specialties": [
      "Propriedades de Luxo",
      "Casas à Beira-Mar",
      "Imóveis Comerciais"
    ],
    "languages": ["Inglês", "Espanhol"],
    "social_media": {
      "facebook": "https://facebook.com/johnsmith",
      "instagram": "https://instagram.com/johnsmith",
      "linkedin": "https://linkedin.com/in/johnsmith"
    },
    "created_at": "2024-01-15T08:00:00Z",
    "updated_at": "2025-10-16T10:30:00Z"
  }
}
```

## Estrutura JSON Explicada

| Campo        | Tipo     | Descrição |
| ------------ | -------- | --------- |
| data | object | Detalhes do agente |
| data.uuid | string | Identificador único do agente |
| data.name | string | Nome completo do agente |
| data.email | string | Endereço de e-mail do agente |
| data.phone | string | Número de telefone do escritório do agente |
| data.mobile | string | Número de telefone celular do agente |
| data.bio | string | Biografia/descrição do agente |
| data.agency | object | Informações da agência |
| data.agency.uuid | string | Identificador único da agência |
| data.agency.name | string | Nome da agência |
| data.agency.address | string | Endereço físico da agência |
| data.agency.website | string | URL do site da agência |
| data.avatar | string | URL da foto de perfil do agente |
| data.properties_count | integer | Número de propriedades ativas gerenciadas pelo agente |
| data.specialties[] | array | Lista de especialidades do agente |
| data.languages[] | array | Idiomas falados pelo agente |
| data.social_media | object | Links de perfis de redes sociais |
| data.created_at | string | Data/hora de registro do agente (ISO 8601) |
| data.updated_at | string | Data/hora da última atualização do agente (ISO 8601) |

## Status HTTP

- 200: Sucesso
- 404: Não encontrado
- 429: Limite de requests excedido
- 500: Erro interno

## Erros

```json
{
  "message": "Agente não encontrado"
}
```

## Notas

- O agente deve pertencer à plataforma especificada pelo cabeçalho `X-PUBLIC-KEY`
- Todos os campos traduzíveis respeitam o cabeçalho `Accept-Language`
- Os links de redes sociais são opcionais e podem ser nulos
- O `properties_count` inclui apenas propriedades ativas/publicadas

## Relacionados

- [Listar Agentes](AgentIndex.md)
- [Listar Propriedades](PropertyIndex.md)

## Changelog

- 2025-10-16: Documentação inicial
