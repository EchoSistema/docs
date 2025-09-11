# Usuários da Plataforma — Contador

- Método: `GET`
- Rota: `/api/v1/ia/admin/users/counter`
- Auth: `auth:sanctum`

## Resumo
Retorna a quantidade de usuários por role na plataforma atual, respeitando a visibilidade mínima de role do solicitante.

## Query Params
- Mesmos filtros do Listar (role/usuário/ocupação) para restringir o escopo da contagem.

## Resposta

```json
{
  "data": {
    "counter": {
      "guest": {
        "role_id": 5,
        "name": "guest",
        "localized_name": "Convidado",
        "total": 42
      }
    }
  }
}
```

