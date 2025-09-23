# Endereços — Atualizar

## Endpoint

`PUT /ia/admin/addresses/{address}` (IA Admin)

Atualiza um endereço existente. Apenas campos enviados são alterados; `uuid` é proibido.

---

## Autenticação

`auth:sanctum`.

### Cabeçalhos Necessários
| Cabeçalho | Tipo | Descrição |
| --------- | ---- | --------- |
| Authorization | `string` | `Bearer <token>` obrigatório. |
| Accept-Language | `string` | Locale (ex.: `en`, `pt-BR`). Opcional. |

---

## Corpo (JSON)
| Campo           | Tipo              | Req. | Descrição |
| --------------- | ----------------- | ---- | --------- |
| `type`          | `AddressTypeEnum` | Não  | Tipo do endereço. |
| `city`          | `string|object`   | Não  | Referência de cidade ou usar IDs. |
| `city_id`       | `integer`         | Não  | ID da cidade. |
| `state_id`      | `integer`         | Não  | ID do estado. |
| `country_id`    | `integer`         | Não  | ID do país. |
| `zipcode`       | `string`          | Não  | CEP. |
| `address_one`   | `string`          | Não  | Linha 1. |
| `address_two`   | `string`          | Não  | Linha 2. |
| `address_three` | `string`          | Não  | Linha 3. |
| `address_four`  | `string`          | Não  | Linha 4. |
| `uuid`          | —                 | —    | Proibido. |

### Sucesso (200)
Retorna o recurso atualizado.

### Erro de Validação (422)
```json
{ "message": "The given data was invalid.", "errors": { "uuid": ["The uuid field is prohibited."], "city_id": ["The city id must be an integer."], "zipcode": ["The zipcode must be a string."] } }
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
- Localização: nomes de cidade/estado/país podem seguir `Accept-Language`.

