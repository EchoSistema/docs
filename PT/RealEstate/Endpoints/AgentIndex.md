# RealEstate – Listar Agentes Imobiliários

## Endpoint

```
GET /api/v1/real-estate/agents
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
| per_page | integer | Não | Itens por página | 9 (máx: 200) |
| page | integer | Não | Número da página | 1 |

## Exemplos

### Exemplo de requisição (curl)

```bash
curl -X GET \
  -H "X-PUBLIC-KEY: <chave>" \
  -H "Accept-Language: pt-BR" \
  "https://sandbox.seu-dominio.com/api/v1/real-estate/agents?per_page=9"
```

### Exemplo de resposta

```json
{
  "data": [
    {
      "uuid": "660e8400-e29b-41d4-a716-446655440001",
      "name": "John Smith",
      "email": "john.smith@agency.com",
      "phone": "+1-555-0123",
      "bio": "Agente imobiliário experiente com mais de 10 anos no mercado de Miami",
      "agency": {
        "uuid": "770e8400-e29b-41d4-a716-446655440002",
        "name": "Premium Real Estate"
      },
      "avatar": "https://example.com/agents/john-smith.jpg",
      "properties_count": 25,
      "created_at": "2024-01-15T08:00:00Z"
    }
  ],
  "links": {
    "first": "https://sandbox.seu-dominio.com/api/v1/real-estate/agents?page=1",
    "last": "https://sandbox.seu-dominio.com/api/v1/real-estate/agents?page=3",
    "prev": null,
    "next": "https://sandbox.seu-dominio.com/api/v1/real-estate/agents?page=2"
  },
  "meta": {
    "current_page": 1,
    "from": 1,
    "last_page": 3,
    "per_page": 9,
    "to": 9,
    "total": 25
  }
}
```

## Estrutura JSON Explicada

| Campo        | Tipo     | Descrição |
| ------------ | -------- | --------- |
| data[] | array | Lista de agentes imobiliários |
| data[].uuid | string | Identificador único do agente |
| data[].name | string | Nome completo do agente |
| data[].email | string | Endereço de e-mail do agente |
| data[].phone | string | Número de telefone do agente |
| data[].bio | string | Biografia/descrição do agente |
| data[].agency | object | Informações da agência |
| data[].agency.uuid | string | Identificador único da agência |
| data[].agency.name | string | Nome da agência |
| data[].avatar | string | URL da foto de perfil do agente |
| data[].properties_count | integer | Número de propriedades gerenciadas pelo agente |
| data[].created_at | string | Data/hora de registro do agente (ISO 8601) |
| links | object | Links de paginação |
| meta | object | Metadados de paginação |

## Status HTTP

- 200: Sucesso
- 400: Requisição inválida
- 422: Erro de validação
- 429: Limite de requests excedido
- 500: Erro interno

## Erros

```json
{
  "message": "Erro de validação",
  "errors": {
    "per_page": ["O campo per page não pode ser maior que 200."]
  }
}
```

## Notas

- O parâmetro `public_key` é extraído automaticamente do cabeçalho `X-PUBLIC-KEY`
- Os resultados são paginados com padrão de 9 itens por página (máx 200)
- Apenas agentes associados à plataforma são retornados
- O campo `properties_count` mostra as propriedades ativas de cada agente

## Relacionados

- [Exibir Detalhes do Agente](AgentShow.md)
- [Listar Propriedades](PropertyIndex.md)

## Changelog

- 2025-10-16: Documentação inicial
