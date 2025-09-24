# IA — Preço por Byte: Atualizar

## Endpoint

```
PUT /ia/admin/pricing/bytes/{product}
```

Atualiza a descrição e/ou o preço único de um produto BYTE. O título não é atualizável.

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

### Corpo da requisição

```json
{
  "price": 150,
  "currency": "EUR",
  "description": "Nova descrição"
}
```

| Campo       | Tipo    | Obrigatório | Descrição |
| ----------- | ------- | ----------- | --------- |
| price       | integer | Não         | Novo valor do preço (menor unidade; ex.: centavos). |
| currency    | string  | Não (obrigatório com price) | Código da moeda (USD, EUR, BRL, PYG). |
| description | string  | Não         | Nova descrição padrão. |

## Exemplos

### Requisição (curl)

```bash
curl -X PUT \
  -H "Authorization: Bearer <token>" \
  -H "Accept-Language: pt-BR" \
  -H "Content-Type: application/json" \
  -d '{"price":150,"description":"Nova descrição"}' \
  "https://sandbox.seu-dominio.com/ia/admin/pricing/bytes/{product}"
```

### Resposta (200)

```json
{ "data": { "uuid": "...", "measurement_type": "byte" } }
```

## Status HTTP

- 200: Sucesso
- 400: Requisição inválida
- 401: Não autenticado
- 404: Não encontrado (quando o produto não for BYTE)
- 422: Erro de validação

## Notas

- O título para Preço por Byte é imutável e não será atualizado por este endpoint.

## Relacionados

- docs/PT/IA/Endpoints/BytePricingIndex.md
- docs/PT/IA/Endpoints/BytePricingShow.md
- docs/PT/IA/Endpoints/BytePricingStore.md
- docs/PT/IA/Endpoints/BytePricingDestroy.md

## Changelog

- 2025-09-23: Atualizado para preço único e título imutável.
