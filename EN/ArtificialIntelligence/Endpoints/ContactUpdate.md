# Inteligência Artificial – Atualizar Contato

## Endpoint

```
PUT /api/v1/ai/admin/contacts/{contact}
```

Atualiza as informações de um contato existente.

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

### Parâmetros de rota

| Parâmetro | Tipo   | Obrigatório | Descrição |
| --------- | ------ | ----------- | --------- |
| contact   | string/int | Sim     | UUID ou ID do contato a ser atualizado. |

### Parâmetros do corpo

| Parâmetro    | Tipo   | Obrigatório | Descrição | Padrão/Valores |
| ------------ | ------ | ----------- | --------- | -------------- |
| type         | string | Não         | Tipo de contato. | Valores válidos: `EMAIL`, `PHONE`, `MOBILE`, `FAX`, `WHATSAPP`, `TELEGRAM`, `OTHER` |
| value        | string | Não         | Valor do contato (email, telefone, etc.). | - |
| is_primary   | boolean| Não         | Se é o contato principal. | - |
| is_verified  | boolean| Não         | Se o contato foi verificado. | - |
| notes        | string | Não         | Notas adicionais sobre o contato. | - |

> Todos os campos são opcionais. Apenas os campos enviados serão atualizados.

> Os nomes dos parâmetros aceitam variantes `snake_case`, `camelCase`, `kebab-case` ou `CapitalCase`.

## Exemplos

### Exemplo de requisição (curl)

```bash
curl -X PUT \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: pt-BR" \
  -H "Content-Type: application/json" \
  -d '{
    "value": "novo-email@exemplo.com.br",
    "is_verified": true,
    "notes": "Email atualizado e verificado"
  }' \
  "https://your-domain.com/api/v1/ai/admin/contacts/550e8400-e29b-41d4-a716-446655440000"
```

### Exemplo de requisição (JavaScript)

```javascript
const contactId = '550e8400-e29b-41d4-a716-446655440000';

const response = await fetch(`https://your-domain.com/api/v1/ai/admin/contacts/${contactId}`, {
  method: 'PUT',
  headers: {
    'Authorization': 'Bearer <token>',
    'X-PUBLIC-KEY': '<key>',
    'Accept-Language': 'pt-BR',
    'Content-Type': 'application/json',
  },
  body: JSON.stringify({
    is_primary: true,
    notes: 'Contato principal atualizado'
  })
});

const result = await response.json();
```

### Exemplo de requisição (PHP)

```php
<?php

use Illuminate\Support\Facades\Http;

$contactId = '550e8400-e29b-41d4-a716-446655440000';

$response = Http::withHeaders([
    'Authorization' => 'Bearer ' . $token,
    'X-PUBLIC-KEY' => $publicKey,
    'Accept-Language' => 'pt-BR',
])->put("https://your-domain.com/api/v1/ai/admin/contacts/{$contactId}", [
    'value' => 'novo-email@exemplo.com.br',
    'is_verified' => true,
]);

$result = $response->json();
```

## Resposta

### Sucesso `200 OK`

```json
{
  "message": "Contato atualizado com sucesso.",
  "data": {
    "contact": {
      "uuid": "550e8400-e29b-41d4-a716-446655440000",
      "type": "EMAIL",
      "value": "novo-email@exemplo.com.br",
      "is_primary": true,
      "is_verified": true,
      "notes": "Email atualizado e verificado",
      "created_at": "2025-01-07T15:30:00Z",
      "updated_at": "2025-01-07T16:45:00Z"
    }
  }
}
```

### Erro `404 Not Found`

```json
{
  "message": "Contato não encontrado."
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

* Apenas os campos fornecidos no corpo da requisição serão atualizados.
* O campo `value` é validado de acordo com o tipo (ex.: formato de email para tipo EMAIL).
* Se `is_primary` for definido como `true`, outros contatos do mesmo usuário/entidade serão desmarcados automaticamente.
* Atualização para `is_verified: true` pode requerer permissões especiais.

## Referências

* Controller: `src/Domain/Shared/Http/Controllers/ContactController.php`
* Rota: `src/Domain/ArtificialIntelligence/routes/api.php:33`
