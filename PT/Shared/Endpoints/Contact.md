# Contatos — Criar/Atualizar (IA Admin)

## Endpoints

- `POST /ia/admin/contacts`
- `PUT /ia/admin/contacts/{contact}`

Cria ou atualiza um contato do usuário (email ou telefone: telephone, whatsapp, telegram).

---

## Autenticação

Obrigatória — `auth:sanctum`.

### Cabeçalhos Necessários
| Cabeçalho | Tipo | Descrição |
| --------- | ---- | --------- |
| Authorization | `string` | `Bearer <token>` obrigatório. |
| Accept-Language | `string` | Locale para campos traduzíveis (ex.: `en`, `pt-BR`). Opcional. |

---

## POST /ia/admin/contacts

Cria um contato vinculado a um usuário por UUID.

### Corpo (JSON)
| Campo          | Tipo                 | Obs. | Descrição |
| -------------- | -------------------- | ---- | --------- |
| `uuid`         | `uuid`               | Sim  | UUID do usuário ao qual o contato será vinculado. |
| `type`         | `ContactTypeEnum`    | Sim  | Um de `email`, `telephone`, `whatsapp`, `telegram`. |
| `email`        | `email`              | Cond | Obrigatório se `type = email` (ou use `value`). |
| `value`        | `email`              | Cond | Alternativa a `email` quando `type = email`. |
| `country_code` | `string`             | Cond | Obrigatório com `number` quando `type` não é `email`. Aceita `+` e dígitos (outros ignorados). |
| `number`       | `string`             | Cond | Número (somente dígitos) quando `type` não é `email`. |

Validação exige campos de email (para `email`) ou de telefone (para demais tipos).

### Exemplos de Requisição
Contato de email:
```json
{
  "uuid": "11111111-1111-1111-1111-111111111111",
  "type": "email",
  "email": "john.doe@example.com"
}
```

Contato WhatsApp:
```json
{
  "uuid": "11111111-1111-1111-1111-111111111111",
  "type": "whatsapp",
  "country_code": "+55",
  "number": "11 91234-5678"
}
```

### Respostas
- `200 OK` com o recurso criado.

### Exemplos de Resposta
Email:
```json
{ "email": "john.doe@example.com" }
```

WhatsApp/Telephone/Telegram:
```json
{
  "whatsapp": {
    "country_code": "+55",
    "number": "11912345678",
    "full": "+55 11912345678"
  }
}
```

### Erro de Validação (422)
Exemplo quando campos obrigatórios estão ausentes ou inválidos para o `type` escolhido.
```json
{
  "message": "The given data was invalid.",
  "errors": {
    "uuid": ["The uuid field must be a valid UUID."],
    "email": ["The email field is required when type is email."],
    "country_code": ["The country code field is required when type is not email."],
    "number": ["The number field is required when type is not email."]
  }
}
```

### Não Encontrado (404)
Quando o UUID de usuário informado não existe.
```json
{ "message": "Usuário não encontrado." }
```

### Não Autenticado (401)
Quando falta um token Sanctum válido.
```json
{ "message": "Unauthenticated." }
```

### Proibido (403)
Quando o usuário autenticado não tem permissão para esta ação.
```json
{ "message": "This action is unauthorized." }
```

---

## PUT /ia/admin/contacts/{contact}

Atualiza um contato existente. Campos são opcionais e somente os enviados são atualizados. Ao alterar `type`, a estrutura armazenada se adapta.

### Corpo (JSON)
| Campo          | Tipo              | Obs. | Descrição |
| -------------- | ----------------- | ---- | --------- |
| `type`         | `ContactTypeEnum` | Não  | Altera o tipo (`email`, `telephone`, `whatsapp`, `telegram`). |
| `email`        | `email`           | Não  | Email (para `email`) ou use `value`. |
| `value`        | `email`           | Não  | Alternativa a `email` quando `type = email`. |
| `country_code` | `string`          | Não  | `+` e dígitos; não dígitos são ignorados. |
| `number`       | `string`          | Não  | Somente dígitos; não dígitos são ignorados. |
| `uuid`         | —                 | —    | Proibido (não é possível trocar o usuário). |

### Respostas
- `200 OK` com o recurso atualizado.

### Erro de Validação (422)
Exemplo ao enviar campos inválidos ou tentar alterar atributo proibido.
```json
{
  "message": "The given data was invalid.",
  "errors": {
    "email": ["The email must be a valid email address."],
    "uuid": ["The uuid field is prohibited."]
  }
}
```

### Não Encontrado (404)
Quando o contato não existe.
```json
{ "message": "Not Found" }
```

### Não Autenticado (401)
Token ausente ou inválido.
```json
{ "message": "Unauthenticated." }
```

### Proibido (403)
Autenticado porém sem permissão para atualizar.
```json
{ "message": "This action is unauthorized." }
```

---

## Notas
- Para contatos telefônicos, `country_code` e `number` são saneados; `country_code` é armazenado com `+`.
- O formato de retorno depende do `type`: `{ "email": "..." }` ou `{ "<type>": { country_code, number, full } }`.
- Localização: se existirem campos traduzíveis em dados relacionados, seus valores seguem o cabeçalho `Accept-Language` quando disponível; caso contrário, retornos padrão serão usados.
