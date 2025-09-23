# Contatos — Atualizar (IA Admin)

## Endpoint

`PUT /ia/admin/contacts/{contact}`

Atualiza um contato existente.

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
| `type`         | `ContactTypeEnum` | Não  | `email`, `telephone`, `whatsapp`, `telegram`. |
| `email`        | `email`           | Não  | Para tipo `email` (ou usar `value`). |
| `value`        | `email`           | Não  | Alternativa a `email` para tipo `email`. |
| `country_code` | `string`          | Não  | `+` e dígitos; demais ignorados. |
| `number`       | `string`          | Não  | Somente dígitos. |
| `uuid`         | —                 | —    | Proibido. |

### Sucesso (200)
Retorna o recurso atualizado no mesmo formato da criação.

### Erro de Validação (422)
```json
{ "message": "The given data was invalid.", "errors": { "email": ["The email must be a valid email address."], "uuid": ["The uuid field is prohibited."] } }
```

### Não Encontrado (404)
```json
{ "message": "Not Found" }
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
- Localização: segue `Accept-Language` quando aplicável.

