# ReputationBook – Listar Status de Reclamações

## Endpoint

```
GET /api/v1/complaints/statuses
```

## Autenticação

Nenhuma

## Cabeçalhos

| Cabeçalho        | Tipo   | Obrigatório | Descrição |
| ---------------- | ------ | ----------- | --------- |
| X-PUBLIC-KEY     | string | Sim         | Chave pública da plataforma. |
| Accept-Language  | string | Não         | Locale IETF (ex.: `pt-BR`, `en`, `es`). |

## Parâmetros

Nenhum parâmetro necessário.

## Exemplos

### Exemplo de requisição (curl)

```bash
curl -X GET \
  -H "X-PUBLIC-KEY: <chave>" \
  -H "Accept-Language: pt-BR" \
  "https://sandbox.seu-dominio.com/api/v1/complaints/statuses"
```

### Exemplo de resposta

```json
{
  "data": [
    {
      "value": "pending",
      "name": "PENDING",
      "title": "Pendente"
    },
    {
      "value": "in_progress",
      "name": "IN_PROGRESS",
      "title": "Em Andamento"
    },
    {
      "value": "resolved",
      "name": "RESOLVED",
      "title": "Resolvido"
    },
    {
      "value": "rejected",
      "name": "REJECTED",
      "title": "Rejeitado"
    }
  ]
}
```

## Estrutura JSON Explicada

| Campo        | Tipo    | Descrição |
| ------------ | ------- | --------- |
| data[]       | array   | Lista de status de reclamações disponíveis. |
| data[].value | string  | O valor do status usado em requisições API. |
| data[].name  | string  | O nome constante interno do status. |
| data[].title | string  | Título localizado para exibição do status. |

## Status HTTP

- 200: Sucesso

## Erros

Não há casos de erro específicos para este endpoint.

## Notas

- Este endpoint retorna todas as opções de status de reclamação disponíveis.
- O campo `title` é localizado com base no cabeçalho `Accept-Language`.
- Nenhuma autenticação é necessária para recuperar a lista de status.

## Relacionados

- [Índice de Reclamações](ComplaintIndex.md)
- [Atualizar Status de Reclamação](ComplaintUpdateStatus.md)
