# IA — Preço por Byte: Detalhe

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
    "uuid": "...",
    "measurement_type": "byte",
    "title": { "content": "Preço por Byte" },
    "text": { "content": "Preço por byte" },
    "prices": [ { "currency_id": 2, "value": 123 } ]
  }
}
```

## Status HTTP

- 200: Sucesso
- 401: Não autenticado
- 404: Não encontrado (quando o produto não for BYTE)

## Relacionados

- docs/PT/IA/Endpoints/BytePricingIndex.md
- docs/PT/IA/Endpoints/BytePricingStore.md
- docs/PT/IA/Endpoints/BytePricingUpdate.md
- docs/PT/IA/Endpoints/BytePricingDestroy.md

## Changelog

- 2025-09-23: Campos e códigos atualizados.
