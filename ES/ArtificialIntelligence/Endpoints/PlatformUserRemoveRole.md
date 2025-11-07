# Inteligência Artificial – Remover Papel de Usuário

## Endpoint

```
DELETE /api/v1/ai/admin/users/{user}/roles/{role}
```

Remove um papel (role) de um usuário da plataforma.

## Autenticação

Obrigatório – Bearer {token} com middleware `auth:sanctum`

## Cabeçalhos

| Cabeçalho        | Tipo   | Obrigatório | Descrição |
| ---------------- | ------ | ----------- | --------- |
| Authorization    | string | Sim         | `Bearer {token}`. |
| X-PUBLIC-KEY     | string | Sim         | Chave pública da plataforma. |
| Accept-Language  | string | Não         | Locale IETF (ex.: `pt-BR`, `en`, `es`). |

## Parâmetros

### Parâmetros de rota

| Parâmetro | Tipo   | Obrigatório | Descrição |
| --------- | ------ | ----------- | --------- |
| user      | string | Sim         | UUID do usuário. |
| role      | string/int | Sim     | Nome ou ID do papel (role) a ser removido. |

## Exemplos

### Exemplo de requisição (curl)

```bash
curl -X DELETE \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: pt-BR" \
  "https://your-domain.com/api/v1/ai/admin/users/550e8400-e29b-41d4-a716-446655440000/roles/admin"
```

### Exemplo de requisição (JavaScript)

```javascript
const userId = '550e8400-e29b-41d4-a716-446655440000';
const roleName = 'admin';

const response = await fetch(`https://your-domain.com/api/v1/ai/admin/users/${userId}/roles/${roleName}`, {
  method: 'DELETE',
  headers: {
    'Authorization': 'Bearer <token>',
    'X-PUBLIC-KEY': '<key>',
    'Accept-Language': 'pt-BR',
  }
});

const result = await response.json();
```

### Exemplo de requisição (PHP)

```php
<?php

use Illuminate\Support\Facades\Http;

$userId = '550e8400-e29b-41d4-a716-446655440000';
$roleName = 'admin';

$response = Http::withHeaders([
    'Authorization' => 'Bearer ' . $token,
    'X-PUBLIC-KEY' => $publicKey,
    'Accept-Language' => 'pt-BR',
])->delete("https://your-domain.com/api/v1/ai/admin/users/{$userId}/roles/{$roleName}");

$result = $response->json();
```

## Resposta

### Sucesso `200 OK`

```json
{
  "message": "Papel removido com sucesso.",
  "data": {
    "user": {
      "uuid": "550e8400-e29b-41d4-a716-446655440000",
      "name": "João Silva",
      "email": "joao@exemplo.com",
      "roles": [
        {
          "id": 2,
          "name": "user",
          "localized_name": "Usuário",
          "platform": {
            "uuid": "11111111-1111-1111-1111-111111111111",
            "name": "Plataforma AI",
            "public_key": "22222222-2222-2222-2222-222222222222"
          }
        }
      ]
    }
  }
}
```

### Erro `404 Not Found` (Usuário)

```json
{
  "message": "Usuário não encontrado."
}
```

### Erro `404 Not Found` (Papel)

```json
{
  "message": "Papel não encontrado ou usuário não possui este papel."
}
```

### Erro `401 Unauthorized`

```json
{
  "message": "Não autenticado."
}
```

### Erro `403 Forbidden`

```json
{
  "message": "Você não tem permissão para remover este papel."
}
```

### Erro `422 Unprocessable Entity`

```json
{
  "message": "Não é possível remover o último papel do usuário.",
  "errors": {
    "role": ["O usuário deve ter pelo menos um papel ativo."]
  }
}
```

## Estrutura JSON

| Campo                        | Tipo    | Descrição |
| ---------------------------- | ------- | --------- |
| `message`                    | string  | Mensagem de sucesso ou erro. |
| `data.user.uuid`             | string  | UUID do usuário. |
| `data.user.name`             | string  | Nome do usuário. |
| `data.user.email`            | string  | Email do usuário. |
| `data.user.roles`            | array   | Lista de papéis restantes do usuário. |
| `data.user.roles[].id`       | int     | ID do papel. |
| `data.user.roles[].name`     | string  | Nome técnico do papel. |
| `data.user.roles[].localized_name` | string | Nome localizado do papel. |
| `data.user.roles[].platform` | object  | Informações da plataforma associada. |

## Notas

* O usuário deve ter pelo menos um papel ativo. Não é possível remover o último papel.
* A remoção de papéis pode afetar as permissões e acessos do usuário imediatamente.
* Alguns papéis podem ter restrições e não podem ser removidos sem permissões especiais.
* O parâmetro `role` aceita tanto o nome técnico (`admin`) quanto o ID numérico do papel.

## Referências

* Controller: `src/Domain/Shared/Http/Controllers/Platform/PlatformUserController.php`
* Rota: `src/Domain/ArtificialIntelligence/routes/api.php:22`
