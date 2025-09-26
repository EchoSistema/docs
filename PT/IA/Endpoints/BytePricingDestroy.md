# IA — Preço por Byte: Excluir

## Endpoint

```
DELETE /ia/admin/pricing/bytes/{product}
```

Exclui (soft-delete) um produto precificado por byte e retorna mensagem de sucesso.

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
{
  "success": true,
  "message": "O byte pricing foi excluído com sucesso"
}
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

## Notas

- A mensagem é localizada via `common.successfully_deleted` com o atributo `byte pricing`.

## Changelog

- 2025-09-23: Adicionados cabeçalhos e parâmetros conforme template.
- 2025-09-26: Resposta ajustada e nota de localização adicionada.
