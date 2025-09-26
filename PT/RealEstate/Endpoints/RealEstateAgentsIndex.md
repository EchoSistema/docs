# Imobiliário — Corretores: Listagem

## Endpoint

```
GET /real-estate/agents
```

## Autenticação

Não é necessária.

## Cabeçalhos

| Cabeçalho       | Tipo   | Obrigatório | Descrição |
| --------------- | ------ | ----------- | --------- |
| Accept-Language | string | Não         | Localiza campos textuais quando houver tradução. |

## Parâmetros de Consulta

| Parâmetro  | Tipo   | Obrigatório | Descrição | Padrão |
| ---------- | ------ | ----------- | --------- | ------ |
| public_key | string | Sim         | Chave pública da plataforma (`pbk`). |
| per_page   | int    | Não         | Tamanho da página (1–200). | 9 |
| page       | int    | Não         | Página desejada. | 1 |

## Exemplo

### Requisição

```bash
curl -X GET "https://api.exemplo.com/real-estate/agents?public_key=realestate-demo&per_page=9"
```

### Resposta (200)

```json
{
  "current_page": 1,
  "data": [
    {
      "uuid": "0f7e3d9a-2a19-4e4b-b0a3-3fb2ef25d001",
      "name": "Laura Nogueira",
      "email": "laura@agency.com",
      "avatar": "https://cdn.exemplo.com/agents/laura.png",
      "phones": ["+55 11 99999-0001"],
      "masked_phones": ["+55 (11) •••••-0001"],
      "role": "senior-agent",
      "properties": 42
    }
  ],
  "per_page": 9,
  "total": 42
}
```

## Códigos de Status

- 200 — Sucesso
- 400 — Parâmetros de paginação inválidos
- 404 — Plataforma não encontrada

## Notas

- Todos os resultados pertencem ao `public_key` informado.
- Os campos são provenientes do MongoDB e podem variar conforme o tenant.

## Changelog

- 2025-09-25 — Documentação inicial.
