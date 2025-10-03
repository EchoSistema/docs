# IA — Preço por Byte: Detalhe

Retorna o produto precificado por byte da plataforma com metadados localizados e preço padrão por byte.

## Endpoint

```
GET /ia/admin/pricing/bytes/details
```

Retorna o produto medido em BYTE cadastrado para a plataforma.

## Autenticação

Obrigatória — Bearer {token} com Sanctum.

## Cabeçalhos

| Cabeçalho        | Tipo   | Obrigatório | Descrição |
| ---------------- | ------ | ----------- | --------- |
| Authorization    | string | Sim         | `Bearer {token}` |
| Accept-Language  | string | Não         | Locale IETF |

## Parâmetros

### Parâmetros de caminho

Nenhum.

## Exemplos

### Requisição (curl)

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "Accept-Language: pt-BR" \
  "https://sandbox.seu-dominio.com/ia/admin/pricing/bytes/details"
```

### Resposta (200)

```json
{
  "data": {
    "uuid": "9e3c5352-a2d7-411d-9ba5-c29756966ca7",
    "measurement_type": {
      "id": "byte",
      "name": "BYTE",
      "title": "Byte"
    },
    "title": "Preço por Byte",
    "slug": "byte_price",
    "description": "Descrição padrão de preço por byte",
    "language": "pt-BR",
    "price": "0.0299",
    "raw_price": 299,
    "price_precision": 4,
    "prices": [
      {
        "currency_id": 840,
        "currency": "USD",
        "value": "0.0055",
        "raw_value": 55,
        "formatted_value": "$0.0055"
      }
    ],
    "currency": "BRL",
    "formatted_price": "R$\u00a00.0299",
    "created_at": "2025-09-26T04:46:04-03:00"
  }
}
```

## Estrutura JSON Explicada

| Campo                         | Tipo        | Descrição |
| ----------------------------- | ----------- | --------- |
| data.uuid                     | string      | Identificador do produto |
| data.measurement_type         | object      | Metadados do tipo de medida |
| data.measurement_type.id      | string      | Identificador enum (`byte`) |
| data.measurement_type.name    | string      | Nome do enum (`BYTE`) |
| data.measurement_type.title   | string      | Rótulo legível |
| data.title                    | string|null | Título localizado do produto |
| data.slug                     | string|null | Slug usado internamente |
| data.description              | string|null | Descrição localizada padrão |
| data.language                 | string|null | Locale associado ao título padrão |
| data.price                    | string|null | Preço por byte com quatro casas decimais |
| data.raw_price                | integer|null | Valor original armazenado na menor unidade |
| data.price_precision          | integer|null | Precisão decimal aplicada |
| data.prices                   | array       | Lista de preços alternativos ativos por moeda |
| data.prices[].currency_id     | integer     | Identificador da moeda |
| data.prices[].currency        | string      | Código ISO da moeda |
| data.prices[].value           | string      | Preço por byte alternativo com quatro casas decimais |
| data.prices[].raw_value       | integer     | Valor alternativo armazenado na menor unidade |
| data.prices[].formatted_value | string      | Preço alternativo formatado com quatro casas decimais |
| data.currency                 | string|null | Código ISO da moeda padrão |
| data.formatted_price          | string|null | Preço padrão formatado com quatro casas decimais |
| data.created_at               | string|null | Data de criação (ISO 8601) |

## Status HTTP

- 200: Sucesso
- 401: Não autenticado
- 404: Não encontrado (quando o produto não for BYTE)

## Relacionados

- docs/PT/IA/Endpoints/BytePricingIndex.md
- docs/PT/IA/Endpoints/BytePricingStore.md
- docs/PT/IA/Endpoints/BytePricingUpdate.md
- docs/PT/IA/Endpoints/BytePricingDestroy.md

## Notas

- O título permanece imutável após a criação.

## Changelog

- 2025-09-23: Campos e códigos atualizados.
- 2025-10-03: Documentados campos de precisão por byte, preços alternativos e novo endpoint sem parâmetro.
- 2025-09-26: Exemplo de resposta atualizado para refletir o recurso atual.
