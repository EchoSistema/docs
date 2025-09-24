# IA — Preço por Byte: Criar

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

### Resposta (201)

```json
{
  "data": {
    "uuid": "...",
    "measurement_type": "byte",
    "title": { "content": "Preço por Byte" },
    "text": { "content": "Preço por byte" },
    "prices": [ { "currency_id": 2, "value": 100, "is_default": true } ]
  }
}
```

## Status HTTP

- 201: Criado
- 400: Requisição inválida
- 401: Não autenticado
- 422: Erro de validação
- 500: Erro interno

## Notas

- Título criado para todos os idiomas a partir da chave `common.byte_price` e slug fixo `byte_price`.
- O título é imutável para os endpoints de Preço por Byte.

## Relacionados

- docs/PT/IA/Endpoints/BytePricingIndex.md
- docs/PT/IA/Endpoints/BytePricingShow.md
- docs/PT/IA/Endpoints/BytePricingUpdate.md
- docs/PT/IA/Endpoints/BytePricingDestroy.md

## Changelog

- 2025-09-23: Alinhado com entrada de preço único e título imutável.
