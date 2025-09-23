# Endereços — Criar

## Endpoint

`POST /addresses` (Backoffice)  |  `POST /ia/admin/addresses` (IA Admin)

Cria um endereço. Quando não há rotas aninhadas, `uuid` é obrigatório.

---

## Autenticação

- `POST /addresses`: `auth:sanctum` com `ability:backoffice`.
- `POST /ia/admin/addresses`: `auth:sanctum`.

### Cabeçalhos Necessários
| Cabeçalho | Tipo | Descrição |
| --------- | ---- | --------- |
| Authorization | `string` | `Bearer <token>` obrigatório. |
| Accept-Language | `string` | Locale (ex.: `en`, `pt-BR`). Opcional. |

---

## Corpo (JSON)
| Campo           | Tipo              | Req. | Descrição |
| --------------- | ----------------- | ---- | --------- |
| `type`          | `AddressTypeEnum` | Não  | Padrão `both`. `billing`, `shipping`, `both`, `fiscal`, `additional`. |
| `uuid`          | `uuid`            | Cond | Obrigatório sem rota `addressable` aninhada. |
| `city`          | `string|object`   | Sim  | Referência de cidade; resolvida via pipeline. |
| `address_one`   | `string`          | Sim  | Linha 1. |
| `address_two`   | `string`          | Não  | Linha 2. |
| `address_three` | `string`          | Não  | Linha 3. |
| `address_four`  | `string`          | Não  | Linha 4. |
| `zipcode`       | `string`          | Não  | CEP. |

### Sucesso (200)
Retorna o recurso criado.

### Erro de Validação (422)
```json
{ "message": "The given data was invalid.", "errors": { "uuid": ["The uuid field must be a valid UUID."], "city": ["The city field is required."], "address_one": ["The address one field is required."] } }
```

### Não Autenticado (401)
```json
{ "message": "Unauthenticated." }
```

### Proibido (403)
Somente para `POST /addresses` quando faltar a ability `backoffice`.
```json
{ "message": "This action is unauthorized." }
```

---

## Notas
- Resolução de cidade via pipeline.
- `type = both` retorna `main: true`; caso contrário, flags específicas.
- Localização: nomes de cidade/estado/país podem seguir `Accept-Language`.

