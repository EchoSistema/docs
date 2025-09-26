# IA — Preço por Byte: Detalhe

Retorna um produto precificado por byte com metadados localizados e preço padrão.

## Endpoint

```
GET /ia/admin/pricing/bytes/{product}
```

Retorna um produto medido em BYTE.

## Autenticação

Obrigatória — Bearer {token} com Sanctum.

## Cabeçalhos

| Cabeçalho        | Tipo   | Obrigatório | Descrição |
| ---------------- | ------ | ----------- | --------- |
| Authorization    | string | Sim         | `Bearer {token}` |
| Accept-Language  | string | Não         | Locale IETF |

## Parâmetros

### Parâmetros de caminho

| Parâmetro | Tipo   | Obrigatório | Descrição |
| --------- | ------ | ----------- | --------- |
| product   | string | Sim         | Chave de rota do produto |

## Exemplos

### Requisição (curl)

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "Accept-Language: pt-BR" \
  "https://sandbox.seu-dominio.com/ia/admin/pricing/bytes/{product}"
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
    "price": 29900,
    "currency": "BRL",
    "formatted_price": "R$\u00a0299,00",
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
| data.price                    | integer|null | Preço padrão na menor unidade |
| data.currency                 | string|null | Código ISO da moeda |
| data.formatted_price          | string|null | Preço formatado |
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
- 2025-09-26: Exemplo de resposta atualizado para refletir o recurso atual.
