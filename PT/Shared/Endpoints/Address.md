# Endereços — Criar/Atualizar

## Endpoints

- `POST /addresses` (Backoffice)
- `POST /ia/admin/addresses` (IA Admin)
- `PUT /ia/admin/addresses/{address}` (IA Admin)

Cria ou atualiza endereços. Os endpoints compartilham as mesmas regras de payload; o `PUT` atualiza um recurso existente.

---

## Autenticação

- `POST /addresses`: `auth:sanctum` com `ability:backoffice`.
- `POST|PUT /ia/admin/addresses`: `auth:sanctum`.

### Cabeçalhos Necessários
| Cabeçalho | Tipo | Descrição |
| --------- | ---- | --------- |
| Authorization | `string` | `Bearer <token>` obrigatório. |
| Accept-Language | `string` | Locale para campos traduzíveis (ex.: `en`, `pt-BR`). Opcional. |

---

## POST /addresses e POST /ia/admin/addresses

Cria um endereço. Quando usados sem rotas aninhadas, `uuid` é obrigatório para vincular o endereço a uma entidade.

### Corpo (JSON)
| Campo           | Tipo              | Req. | Descrição |
| --------------- | ----------------- | ---- | --------- |
| `type`          | `AddressTypeEnum` | Não  | Padrão `both`. Um de `billing`, `shipping`, `both`, `fiscal`, `additional`. |
| `uuid`          | `uuid`            | Cond | Obrigatório quando não houver rota `addressable` aninhada. |
| `city`          | `string|object`   | Sim  | Referência da cidade; resolvida via pipeline. Pode ser nome ou objeto. |
| `address_one`   | `string`          | Sim  | Linha 1 do endereço. |
| `address_two`   | `string`          | Não  | Linha 2 do endereço. |
| `address_three` | `string`          | Não  | Linha 3 do endereço. |
| `address_four`  | `string`          | Não  | Linha 4 do endereço. |
| `zipcode`       | `string`          | Não  | CEP/código postal. |

### Respostas
- `200 OK` com o recurso criado.

### Exemplo de Resposta
```json
{
  "uuid": "aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa",
  "main": true,
  "zipcode": "01000-000",
  "one": "Av. Paulista, 1000",
  "two": null,
  "three": null,
  "four": null,
  "city": { "uuid": "...", "name": "São Paulo", "abbreviation": "SP" },
  "state": { "uuid": "...", "name": "São Paulo", "abbreviation": "SP", "region": "Sudeste" },
  "country": { "uuid": "...", "name": "Brasil", "official_name": "República Federativa do Brasil", "code": "BR" },
  "formatted": "Av. Paulista, 1000 - São Paulo/SP, 01000-000, Brasil"
}
```

### Erro de Validação (422)
Exemplo quando campos obrigatórios estão ausentes ou inválidos.
```json
{
  "message": "The given data was invalid.",
  "errors": {
    "uuid": ["The uuid field must be a valid UUID."],
    "city": ["The city field is required."],
    "address_one": ["The address one field is required."]
  }
}
```

### Não Autenticado (401)
Quando faltar um token Sanctum válido.
```json
{ "message": "Unauthenticated." }
```

### Proibido (403)
Somente para `POST /addresses` quando o usuário não possui a ability `backoffice`.
```json
{ "message": "This action is unauthorized." }
```

Se `type` for diferente de `both`, a resposta inclui flags:
```json
{
  "uuid": "...",
  "main": false,
  "is_billing": true,
  "is_shipping": false,
  "is_fiscal": false,
  "zipcode": "..."
}
```

---

## PUT /ia/admin/addresses/{address}

Atualiza um endereço existente. Apenas campos enviados são alterados; `uuid` não pode ser modificado.

### Corpo (JSON)
| Campo           | Tipo              | Req. | Descrição |
| --------------- | ----------------- | ---- | --------- |
| `type`          | `AddressTypeEnum` | Não  | Tipo de endereço. |
| `city`          | `string|object`   | Não  | Referência de cidade; pode usar IDs abaixo. |
| `city_id`       | `integer`         | Não  | ID da cidade. |
| `state_id`      | `integer`         | Não  | ID do estado. |
| `country_id`    | `integer`         | Não  | ID do país. |
| `zipcode`       | `string`          | Não  | CEP/código postal. |
| `address_one`   | `string`          | Não  | Linha 1. |
| `address_two`   | `string`          | Não  | Linha 2. |
| `address_three` | `string`          | Não  | Linha 3. |
| `address_four`  | `string`          | Não  | Linha 4. |
| `uuid`          | —                 | —    | Proibido (não altera relacionamento). |

### Respostas
- `200 OK` com o recurso atualizado.

### Erro de Validação (422)
Exemplo ao enviar campos inválidos ou tentar modificar atributos proibidos.
```json
{
  "message": "The given data was invalid.",
  "errors": {
    "uuid": ["The uuid field is prohibited."],
    "city_id": ["The city id must be an integer."],
    "zipcode": ["The zipcode must be a string."]
  }
}
```

### Não Encontrado (404)
Quando o endereço não existe.
```json
{ "message": "Not Found" }
```

### Não Autenticado (401)
Token ausente ou inválido.
```json
{ "message": "Unauthenticated." }
```

### Proibido (403)
Autenticado porém sem permissão para atualizar o recurso.
```json
{ "message": "This action is unauthorized." }
```

---

## Notas
- A resolução de cidade é feita por pipeline; é possível informar nome/objeto ou IDs diretos.
- Quando `type = both`, o recurso marca `main: true`; caso contrário, flags indicam papéis de cobrança/entrega/fiscal.
- Localização: nomes de cidade, estado e país podem ser traduzidos conforme o cabeçalho `Accept-Language` quando houver traduções disponíveis; caso contrário, retornos padrão serão usados.
