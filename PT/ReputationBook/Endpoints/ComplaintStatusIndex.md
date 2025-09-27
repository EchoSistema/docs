# ReputationBook – Listar status de reclamação

## Endpoint

```
GET /api/v1/complaints/statuses
```

Retorna todos os status possíveis (`ComplaintStatusEnum`) com informações de valor e rótulo.

## Autenticação

Nenhuma. Apenas `X-PUBLIC-KEY` é requerido pelo middleware `platform`.

## Cabeçalhos

| Cabeçalho | Tipo | Obrigatório | Descrição |
| --------- | ---- | ----------- | --------- |
| `X-PUBLIC-KEY` | string | Sim | Plataforma requisitante. |
| `Accept-Language` | string | Opcional | Afeta apenas traduções no frontend; o endpoint retorna `title()` já localizado. |

## Exemplo de resposta (200)

```json
{
  "data": [
    {
      "value": "open",
      "name": "OPEN",
      "title": "Aberta"
    },
    {
      "value": "in_progress",
      "name": "IN_PROGRESS",
      "title": "Em andamento"
    },
    {
      "value": "resolved",
      "name": "RESOLVED",
      "title": "Resolvida"
    }
  ]
}
```

## Estrutura JSON explicada

| Campo | Tipo | Descrição |
| ----- | ---- | --------- |
| `data[]` | array | Lista de status disponíveis. |
| `data[].value` | string | Valor persistido (enum). |
| `data[].name` | string | Nome constante da enum (`UPPER_CASE`). |
| `data[].title` | string | Título localizado para exibição. |

## Status HTTP

- 200: Lista retornada.
- 500: Erro inesperado.

