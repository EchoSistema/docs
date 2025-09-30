# IA — Preço por Byte: Criar

Cria um produto precificado por byte com conteúdo localizado e um único preço ativo.

> Branch: master

Se um produto de preço por byte (slug `byte_price`) já existir para a plataforma atual, o endpoint apenas retorna esse registro em vez de criar um duplicado.

## Endpoint

```
POST /ia/admin/pricing/bytes
```

Cria um produto medido em BYTE com título localizado, descrição opcional e um único preço.

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

### Parâmetros de consulta

Nenhum.

### Corpo da requisição

```json
{
  "price": 100,
  "currency": "USD",
  "description": "Preço por byte"
}
```

| Campo       | Tipo    | Obrigatório | Descrição |
| ----------- | ------- | ----------- | --------- |
| price       | integer | Sim         | Valor do preço (menor unidade; ex.: centavos). |
| currency    | string  | Sim         | Código da moeda (USD, EUR, BRL, PYG). |
| description | string  | Não         | Descrição padrão. |

## Exemplos

### Requisição (curl)

```bash
curl -X POST \
  -H "Authorization: Bearer <token>" \
  -H "Accept-Language: pt-BR" \
  -H "Content-Type: application/json" \
  -d '{"price":100,"description":"Preço por byte"}' \
  "https://sandbox.seu-dominio.com/ia/admin/pricing/bytes"
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
    "description": "Preço por byte",
    "language": "pt-BR",
    "price": 100,
    "currency": "USD",
    "formatted_price": "US$\u00a01,00",
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
| data.price                    | integer     | Preço padrão na menor unidade (ex.: centavos) |
| data.currency                 | string      | Código ISO da moeda |
| data.formatted_price          | string|null | Preço formatado |
| data.created_at               | string|null | Data de criação (ISO 8601) |

## Status HTTP

- 200: Sucesso
- 400: Requisição inválida
- 401: Não autenticado
- 422: Erro de validação
- 500: Erro interno

## Notas

- Título criado a partir da chave `common.byte_price`; o slug permanece `byte_price` para integração com o Data Calculator.
- O título é imutável para os endpoints de Preço por Byte.

## Relacionados

- docs/PT/IA/Endpoints/BytePricingIndex.md
- docs/PT/IA/Endpoints/BytePricingShow.md
- docs/PT/IA/Endpoints/BytePricingUpdate.md
- docs/PT/IA/Endpoints/BytePricingDestroy.md

## Changelog

- 2025-09-23: Alinhado com entrada de preço único e título imutável.
- 2025-09-26: Resposta e status ajustados para refletir a implementação atual.
