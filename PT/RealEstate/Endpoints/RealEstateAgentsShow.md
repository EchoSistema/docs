# Imobiliário — Corretores: Detalhe

## Endpoint

```
GET /real-estate/agents/{agent}
```

## Autenticação

Não é necessária.

## Parâmetros de Caminho

| Parâmetro | Tipo | Obrigatório | Descrição |
| --------- | ---- | ----------- | --------- |
| agent     | uuid | Sim         | UUID do corretor. |

## Exemplo

### Requisição

```bash
curl -X GET "https://api.exemplo.com/real-estate/agents/0f7e3d9a-2a19-4e4b-b0a3-3fb2ef25d001"
```

### Resposta (200)

```json
{
  "data": {
    "uuid": "0f7e3d9a-2a19-4e4b-b0a3-3fb2ef25d001",
    "name": "Laura Nogueira",
    "email": "laura@agency.com",
    "avatar": "https://cdn.exemplo.com/agents/laura.png",
    "phones": ["+55 11 99999-0001"],
    "masked_phones": ["+55 (11) •••••-0001"],
    "role": "senior-agent",
    "properties": 42,
    "socials": {
      "instagram": "@laura.moves",
      "linkedin": "laura-nogueira"
    }
  }
}
```

## Códigos de Status

- 200 — Sucesso
- 404 — Corretor não encontrado

## Notas

- Os dados são lidos diretamente da coleção MongoDB `agents`.
- O UUID já é único por tenant, portanto `public_key` é opcional.

## Changelog

- 2025-09-25 — Documentação inicial.
