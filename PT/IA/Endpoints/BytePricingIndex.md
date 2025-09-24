# IA — Preço por Byte: Listagem

## Endpoint

```
GET /ia/admin/pricing/bytes
```

## Autenticação

Obrigatória — Bearer {token} com Sanctum.

## Cabeçalhos

| Cabeçalho        | Tipo   | Obrigatório | Descrição |
| ---------------- | ------ | ----------- | --------- |
| Authorization    | string | Sim         | `Bearer {token}` |
| Accept-Language  | string | Não         | Locale IETF (`pt-BR`, `en`, `es`, ...) |

## Parâmetros

### Parâmetros de caminho

Nenhum.

### Parâmetros de consulta

| Parâmetro | Tipo | Obrigatório | Descrição   | Padrão |
| --------- | ---- | ----------- | ----------- | ------ |
| page      | int  | Não         | Página      | 1      |

## Exemplos

### Requisição (curl)

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "Accept-Language: pt-BR" \
  "https://sandbox.seu-dominio.com/ia/admin/pricing/bytes?page=1"
```

### Resposta (200)

```json
{
  "data": [
    {
      "uuid": "...",
      "measurement_type": "byte",
      "title": { "content": "Preço por Byte" },
      "text": { "content": "Preço por byte" },
      "prices": [ { "currency_id": 2, "value": 123 } ]
    }
  ],
  "meta": { "current_page": 1, "per_page": 25 }
}
```

## Estrutura JSON Explicada

| Campo                    | Tipo   | Descrição |
| ------------------------ | ------ | --------- |
| data[]                   | array  | Lista de produtos |
| data[].uuid              | string | Identificador do produto |
| data[].measurement_type  | string | Tipo de medição (`byte`) |
| data[].title.content     | string | Título padrão localizado |
| data[].text.content      | string | Descrição padrão |
| data[].prices[]          | array  | Preços do produto |
| meta                     | object | Metadados de paginação |

## Status HTTP

- 200: Sucesso
- 401: Não autenticado
- 429: Limite de requests excedido
- 500: Erro interno

## Notas

- Retorna apenas produtos com `measurement_type = byte`.

## Relacionados

- docs/PT/IA/Endpoints/BytePricingShow.md
- docs/PT/IA/Endpoints/BytePricingStore.md
- docs/PT/IA/Endpoints/BytePricingUpdate.md
- docs/PT/IA/Endpoints/BytePricingDestroy.md

## Changelog

- 2025-09-23: Alinhado ao modelo de preço único e título imutável.
