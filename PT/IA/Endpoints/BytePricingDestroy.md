# IA — Preço por Byte: Excluir

## Endpoint

```
DELETE /ia/admin/pricing/bytes/{product}
```

Exclui (soft-delete) um produto BYTE.

## Autenticação

Obrigatória — Bearer {token} com Sanctum.

## Cabeçalhos

| Cabeçalho        | Tipo   | Obrigatório | Descrição |
| ---------------- | ------ | ----------- | --------- |
| Authorization    | string | Sim         | `Bearer {token}` |

## Parâmetros

### Parâmetros de caminho

| Parâmetro | Tipo   | Obrigatório | Descrição |
| --------- | ------ | ----------- | --------- |
| product   | string | Sim         | Chave de rota do produto |

## Exemplos

### Requisição (curl)

```bash
curl -X DELETE \
  -H "Authorization: Bearer <token>" \
  "https://sandbox.seu-dominio.com/ia/admin/pricing/bytes/{product}"
```

### Resposta (200)

```json
{ "message": "Excluído com sucesso." }
```

## Status HTTP

- 200: Sucesso
- 401: Não autenticado
- 404: Não encontrado (quando o produto não for BYTE)

## Relacionados

- docs/PT/IA/Endpoints/BytePricingIndex.md
- docs/PT/IA/Endpoints/BytePricingShow.md
- docs/PT/IA/Endpoints/BytePricingStore.md
- docs/PT/IA/Endpoints/BytePricingUpdate.md

## Changelog

- 2025-09-23: Adicionados cabeçalhos e parâmetros conforme template.
