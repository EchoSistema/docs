# IA — Preço por Byte: Atualizar

Atualiza a descrição padrão e/ou o preço único de um produto precificado por byte existente.

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
    "description": "Nova descrição",
    "language": "pt-BR",
    "price": 150,
    "currency": "EUR",
    "formatted_price": "€1,50",
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
| data.description              | string|null | Descrição padrão atualizada |
| data.language                 | string|null | Locale associado ao título padrão |
| data.price                    | integer|null | Preço atualizado na menor unidade |
| data.currency                 | string|null | Código ISO da moeda |
| data.formatted_price          | string|null | Preço formatado |
| data.created_at               | string|null | Data de criação (ISO 8601) |

## Status HTTP

- 200: Sucesso
- 400: Requisição inválida
- 401: Não autenticado
- 404: Não encontrado (quando o produto não for BYTE)
- 422: Erro de validação

## Notas

- O título para Preço por Byte é imutável e não será atualizado por este endpoint.
- Preço e moeda devem ser enviados em conjunto; omitir ambos mantém os valores atuais.

## Relacionados

- docs/PT/IA/Endpoints/BytePricingIndex.md
- docs/PT/IA/Endpoints/BytePricingShow.md
- docs/PT/IA/Endpoints/BytePricingStore.md
- docs/PT/IA/Endpoints/BytePricingDestroy.md

## Changelog

- 2025-09-23: Atualizado para preço único e título imutável.
- 2025-09-26: Exemplo de resposta e tabela de campos atualizados.
