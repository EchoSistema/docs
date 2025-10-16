# Learn – Obter Contador de Eventos

## Endpoint

```
GET /api/v1/learn/events/counter
```

## Autenticação

Nenhuma

## Cabeçalhos

| Cabeçalho       | Tipo   | Obrigatório | Descrição |
| --------------- | ------ | ----------- | --------- |
| Authorization   | string | Não         | `Bearer {token}` (opcional). |
| X-PUBLIC-KEY    | string | Sim         | Chave pública da plataforma. |
| Accept-Language | string | Não         | Locale IETF (ex.: `pt-BR`, `en`, `es`). |

## Parâmetros

### Parâmetros de consulta

Parâmetros de filtro adicionais estão disponíveis através do EventCounterFilter.

> O endpoint aceita variações de nomes de parâmetros (`camelCase`, `kebab-case`, `CapitalCase`).

## Exemplos

### Exemplo de requisição (curl)

```bash
curl -X GET \
  -H "X-PUBLIC-KEY: <chave>" \
  -H "Accept-Language: pt-BR" \
  "https://sandbox.seu-dominio.com/api/v1/learn/events/counter"
```

### Exemplo de resposta

```json
{
  "data": {
    "total": 47,
    "active": 42,
    "inactive": 5,
    "by_category": {
      "programacao": 25,
      "design": 12,
      "negocios": 10
    }
  }
}
```

## Estrutura JSON Explicada

| Campo                  | Tipo    | Descrição |
| ---------------------- | ------- | --------- |
| data                   | object  | Dados do contador |
| data.total             | integer | Número total de eventos |
| data.active            | integer | Número de eventos ativos |
| data.inactive          | integer | Número de eventos inativos |
| data.by_category       | object  | Contagem de eventos agrupados por categoria |

## Status HTTP

- 200: Sucesso
- 400: Requisição inválida
- 401: Não autenticado
- 403: Proibido
- 404: Não encontrado
- 429: Limite de requests excedido
- 500: Erro interno

## Erros

```json
{
  "message": "Chave de plataforma inválida"
}
```

## Notas

- Retorna contagens agregadas de eventos para a plataforma
- Suporta filtragem através do EventCounterFilter
- Útil para estatísticas de dashboard e análises
- O contador respeita a propriedade da plataforma e conta apenas eventos pertencentes à plataforma autenticada

## Relacionados

- [Listar Eventos](./EventIndex.md)
- [Exibir Evento](./EventShow.md)

## Changelog

- 2025-10-16: Documentação inicial
