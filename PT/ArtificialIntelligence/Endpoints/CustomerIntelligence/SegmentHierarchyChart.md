# Inteligência Artificial – Gráfico de Hierarquia de Segmentos

## Endpoint

```
POST /api/v1/ai/echointel/customer-intelligence/segment-hierarchy-chart
```

Gráfico de Hierarquia de Segmentos utilizando inteligência artificial e machine learning.

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

## Resposta

Consulte a documentação oficial para formato de resposta completo.

## Referências

* [Documentação EchoIntel](https://github.com/EchoSistema/abintel-documentation)
* Controller: `src/Domain/ArtificialIntelligence/Http/Controllers/EchoIntelProxyController.php:115`
