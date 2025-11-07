# Inteligência Artificial – Criar Contato

## Endpoint

```
POST /api/v1/ai/admin/contacts
```

Cria um novo contato associado a um usuário ou entidade.

## Autenticação

Obrigatório – Bearer {token} com middleware `auth:sanctum`

## Cabeçalhos

| Cabeçalho        | Tipo   | Obrigatório | Descrição |
| ---------------- | ------ | ----------- | --------- |
| Authorization    | string | Sim         | `Bearer {token}`. |
| X-PUBLIC-KEY     | string | Sim         | Chave pública da plataforma. |
| Accept-Language  | string | Não         | Locale IETF (ex.: `pt-BR`, `en`, `es`). |
| Content-Type     | string | Sim         | `application/json`. |

## Parâmetros

### Parâmetros do corpo

| Parâmetro    | Tipo   | Obrigatório | Descrição | Padrão/Valores |
| ------------ | ------ | ----------- | --------- | -------------- |
| type         | string | Sim         | Tipo de contato. | Valores válidos: `EMAIL`, `PHONE`, `MOBILE`, `FAX`, `WHATSAPP`, `TELEGRAM`, `OTHER` |
| value        | string | Sim         | Valor do contato (email, telefone, etc.). | - |
| is_primary   | boolean| Não         | Se é o contato principal. | Padrão: `false` |
| is_verified  | boolean| Não         | Se o contato foi verificado. | Padrão: `false` |
| notes        | string | Não         | Notas adicionais sobre o contato. | - |

> Os nomes dos parâmetros aceitam variantes `snake_case`, `camelCase`, `kebab-case` ou `CapitalCase`.

## Exemplos

### Exemplo de requisição (curl)

```bash
curl -X POST \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: pt-BR" \
  -H "Content-Type: application/json" \
  -d '{
    "type": "EMAIL",
    "value": "contato@exemplo.com.br",
    "is_primary": true,
    "is_verified": false
  }' \
  "https://your-domain.com/api/v1/ai/admin/contacts"
```

### Exemplo de requisição (JavaScript)

```javascript
const response = await fetch('https://your-domain.com/api/v1/ai/admin/contacts', {
  method: 'POST',
  headers: {
    'Authorization': 'Bearer <token>',
    'X-PUBLIC-KEY': '<key>',
    'Accept-Language': 'pt-BR',
    'Content-Type': 'application/json',
  },
  body: JSON.stringify({
    type: 'MOBILE',
    value: '+5511987654321',
    is_primary: true,
    is_verified: false,
    notes: 'Celular corporativo'
  })
});

const result = await response.json();
```

### Exemplo de requisição (PHP)

```php
<?php

use Illuminate\Support\Facades\Http;

$response = Http::withHeaders([
    'Authorization' => 'Bearer ' . $token,
    'X-PUBLIC-KEY' => $publicKey,
    'Accept-Language' => 'pt-BR',
])->post('https://your-domain.com/api/v1/ai/admin/contacts', [
    'type' => 'EMAIL',
    'value' => 'contato@exemplo.com.br',
    'is_primary' => true,
    'is_verified' => false,
]);

$result = $response->json();
```

## Resposta

### Sucesso `201 Created`

```json
{
  "message": "Contato criado com sucesso.",
  "data": {
    "contact": {
      "uuid": "550e8400-e29b-41d4-a716-446655440000",
      "type": "EMAIL",
      "value": "contato@exemplo.com.br",
      "is_primary": true,
      "is_verified": false,
      "notes": null,
      "created_at": "2025-01-07T15:30:00Z",
      "updated_at": "2025-01-07T15:30:00Z"
    }
  }
}
```

### Erro `400 Bad Request`

```json
{
  "message": "Os dados fornecidos são inválidos.",
  "errors": {
    "type": ["O campo type é obrigatório."],
    "value": ["O campo value é obrigatório."]
  }
}
```

### Erro `401 Unauthorized`

```json
{
  "message": "Não autenticado."
}
```

### Erro `422 Unprocessable Entity`

```json
{
  "message": "Os dados fornecidos são inválidos.",
  "errors": {
    "type": ["O tipo de contato selecionado é inválido."],
    "value": ["O formato do email é inválido."]
  }
}
```

## Estrutura JSON

| Campo                | Tipo    | Descrição |
| -------------------- | ------- | --------- |
| `message`            | string  | Mensagem de sucesso ou erro. |
| `data.contact.uuid`  | string  | UUID único do contato. |
| `data.contact.type`  | string  | Tipo do contato. |
| `data.contact.value` | string  | Valor do contato. |
| `data.contact.is_primary` | boolean | Indica se é contato principal. |
| `data.contact.is_verified` | boolean | Indica se foi verificado. |
| `data.contact.notes` | string  | Notas adicionais (pode ser null). |
| `data.contact.created_at` | string | Data de criação (ISO 8601). |
| `data.contact.updated_at` | string | Data de atualização (ISO 8601). |

## Notas

* Tipos de contato disponíveis: `EMAIL`, `PHONE`, `MOBILE`, `FAX`, `WHATSAPP`, `TELEGRAM`, `OTHER`.
* O campo `value` é validado de acordo com o tipo (ex.: formato de email para tipo EMAIL).
* Apenas um contato pode ser marcado como `is_primary` por usuário/entidade.
* Contatos verificados (`is_verified: true`) requerem processo de verificação adicional.

## Referências

* Controller: `src/Domain/Shared/Http/Controllers/ContactController.php`
* Rota: `src/Domain/ArtificialIntelligence/routes/api.php:32`
