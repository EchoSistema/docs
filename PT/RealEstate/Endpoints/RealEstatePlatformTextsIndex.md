# Imobiliário — Textos da Plataforma: Listagem

## Endpoint

```
GET /real-estate/platform-texts
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
curl -X GET "https://api.exemplo.com/real-estate/platform-texts?public_key=realestate-demo"
```

### Resposta (200)

```json
{
  "data": [
    {
      "key": "hero.title",
      "text": "Encontre seu próximo lar",
      "pbk": "realestate-demo"
    },
    {
      "key": "hero.subtitle",
      "text": "Imóveis selecionados com corretores verificados",
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

- Os textos ficam na coleção MongoDB `platform_texts` e podem ter conteúdo localizado por chave.
- A resposta retorna todos os registros associados ao `public_key` informado.

## Changelog

- 2025-09-25 — Documentação inicial.
