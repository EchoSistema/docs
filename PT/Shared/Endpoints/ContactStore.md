# Contatos — Criar (IA Admin)

## Endpoint

`POST /ia/admin/contacts`

Cria um contato de usuário (email ou telefônico: telephone, whatsapp, telegram).

---

## Autenticação

Obrigatória — `auth:sanctum`.

### Cabeçalhos Necessários
| Cabeçalho | Tipo | Descrição |
| --------- | ---- | --------- |
| Authorization | `string` | `Bearer <token>` obrigatório. |
| Accept-Language | `string` | Locale (ex.: `en`, `pt-BR`). Opcional. |

---

## Corpo (JSON)
| Campo          | Tipo              | Req. | Descrição |
| -------------- | ----------------- | ---- | --------- |
| `uuid`         | `uuid`            | Sim  | UUID do usuário ao qual o contato será vinculado. |
| `type`         | `ContactTypeEnum` | Sim  | `email`, `telephone`, `whatsapp`, `telegram`. |
| `email`        | `email`           | Cond | Obrigatório se `type = email` (ou use `value`). |
| `value`        | `email`           | Cond | Alternativa a `email` quando `type = email`. |
| `country_code` | `string`          | Cond | Com `number` quando `type` não é `email`. Aceita `+` e dígitos. |
| `number`       | `string`          | Cond | Dígitos quando `type` não é `email`. |

### Exemplos
Email:
```json
{ "uuid": "11111111-1111-1111-1111-111111111111", "type": "email", "email": "john.doe@example.com" }
```
WhatsApp:
```json
{ "uuid": "11111111-1111-1111-1111-111111111111", "type": "whatsapp", "country_code": "+55", "number": "11 91234-5678" }
```

### Sucesso (200)
```json
{ "email": "john.doe@example.com" }
```
ou
```json
{ "whatsapp": { "country_code": "+55", "number": "11912345678", "full": "+55 11912345678" } }
```

### Erro de Validação (422)
```json
{ "message": "The given data was invalid.", "errors": { "uuid": ["The uuid field must be a valid UUID."], "email": ["The email field is required when type is email."], "country_code": ["The country code field is required when type is not email."], "number": ["The number field is required when type is not email."] } }
```

### Não Encontrado (404)
```json
{ "message": "Usuário não encontrado." }
```

### Não Autenticado (401)
```json
{ "message": "Unauthenticated." }
```

### Proibido (403)
```json
{ "message": "This action is unauthorized." }
```

---

## Notas
- Saneamento de `country_code`/`number`; `country_code` persiste com `+`.
- Localização via `Accept-Language` quando aplicável.

