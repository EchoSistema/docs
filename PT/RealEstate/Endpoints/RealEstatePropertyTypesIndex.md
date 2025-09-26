# Imobiliário — Tipos de Imóvel: Listagem

## Endpoint

```
GET /real-estate/property-types
```

## Autenticação

Não é necessária.

## Parâmetros de Consulta

| Parâmetro  | Tipo   | Obrigatório | Descrição |
| ---------- | ------ | ----------- | --------- |
| public_key | string | Sim         | Chave pública da plataforma (`pbk`). |

## Exemplo

### Requisição

```bash
curl -X GET "https://api.exemplo.com/real-estate/property-types?public_key=realestate-demo"
```

### Resposta (200)

```json
{
  "data": [
    {
      "uuid": "7cc944ef-3b77-41a1-9f36-3f0a3c1f9001",
      "slug": "apartment",
      "title": "Apartamento",
      "pbk": "realestate-demo"
    },
    {
      "uuid": "e1840e5b-485d-4ea5-8ea6-64cef0f30102",
      "slug": "house",
      "title": "Casa",
      "pbk": "realestate-demo"
    }
  ],
  "meta": {
    "total": 2
  }
}
```

## Códigos de Status

- 200 — Sucesso
- 404 — Plataforma não encontrada

## Notas

- Os dados são lidos da coleção MongoDB `property_types`.
- A listagem retorna todos os tipos sem paginação.

## Changelog

- 2025-09-25 — Documentação inicial.
