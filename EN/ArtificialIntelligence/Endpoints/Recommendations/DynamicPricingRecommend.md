# Inteligência Artificial – Recomendação de Preço Dinâmico

## Endpoint

```
POST /api/v1/ai/echointel/recommendations/dynamic-pricing
```

Recomendação de Preço Dinâmico utilizando sistemas de recomendação baseados em collaborative e content-based filtering.

## Autenticação

Obrigatório – Bearer {token} com middleware `auth:sanctum`

## Cabeçalhos

| Cabeçalho          | Tipo   | Obrigatório | Descrição |
| ------------------ | ------ | ----------- | --------- |
| Authorization      | string | Sim         | `Bearer {token}`. |
| X-Customer-Api-Id  | string | Condicional | UUID do tenant (v4). |
| X-Secret           | string | Condicional | Secret de 64 caracteres. |
| Accept-Language    | string | Não         | Idioma (`en`, `es`, `pt`). |
| Content-Type       | string | Sim         | `application/json`. |

## Parâmetros

> **Nota:** Os parâmetros aceitam tanto `snake_case` quanto `camelCase`.

Consulte a [documentação oficial EchoIntel](https://github.com/EchoSistema/abintel-documentation) para parâmetros específicos.

## Referências

* [Documentação EchoIntel](https://github.com/EchoSistema/abintel-documentation)
* Controller: `src/Domain/ArtificialIntelligence/Http/Controllers/EchoIntelProxyController.php:212`
