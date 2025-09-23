# Usuários da Plataforma — Contador

- Método: `GET`
- Rota: `/api/v1/ia/admin/users/counter`
- Autenticação: `auth:sanctum`

## Resumo
Retorna a quantidade de usuários por papel dentro da plataforma atual, respeitando a visibilidade mínima de papéis do solicitante.

## Parâmetros de consulta
- Mesmos filtros do Índice (papel/usuário/ocupação) para delimitar o escopo da contagem.

## Resposta

```json
{
  "data": {
    "counter": {
      "guest": {
        "role_id": 5,
        "name": "guest",
        "localized_name": "Guest",
        "total": 42
      }
    }
  }
}
```
