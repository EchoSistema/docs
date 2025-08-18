# Auth – Registrar Usuário

## Endpoint

`POST auth/register`

Registra um novo usuário na plataforma.

---

## Autenticação

Nenhuma. Ainda assim, o cabeçalho `X-PUBLIC-KEY` deve ser enviado.

---

## Requisição

### Cabeçalhos Obrigatórios

| Cabeçalho | Tipo | Descrição |
| --------- | ---- | --------- |
| **X-PUBLIC-KEY** | `string` | Chave pública da plataforma necessária para autenticar e receber respostas. |
| **Accept-Language** | `string` | Idioma para mensagens de erro (ex.: `pt-BR`). Opcional. |

### Corpo

| Campo | Tipo | Obrigatório | Descrição |
| ----- | ---- | ----------- | --------- |
| `collaborator` | `boolean` | Não | Indica se o usuário é colaborador. |
| `device` | `string` | Sim | Dispositivo utilizado para o registro. |
| `name` | `string` | Sim | Nome do usuário. |
| `platform` | `mixed` | Não | Plataforma associada. |
| `gender` | `UserGenderEnum` | Condicional | Obrigatório com `collaborator`. |
| `birth_date` | `date` | Condicional | Obrigatório com `collaborator`. |
| `language` | `LanguageEnum` | Não | Idioma preferido do usuário. |
| `currency` | `CurrencyEnum` | Não | Moeda do usuário. |
| `email` | `string` | Sim | E-mail válido e único na plataforma. |
| `password` | `string` | Sim | Senha (mínimo 8 caracteres). |
| `password_confirmation` | `string` | Sim | Confirmação da senha; deve coincidir com `password`. |
| `roles` | `array` | Condicional | Papéis do usuário; obrigatório com `collaborator`. |
| `roles.*` | `RoleEnum` | Condicional | Cada papel deve ser válido. |
| `nationalities` | `array` | Condicional | Nacionalidades do usuário; obrigatório com `collaborator`. |
| `address` | `array` | Condicional | Endereço do usuário; obrigatório com `collaborator`. |
| `address.city` | `string` | Não | Cidade. |
| `address.state` | `string` | Não | Estado. |
| `address.country` | `string` | Não | País. |
| `address.city_id` | `integer` | Não | ID da cidade. |
| `address.state_id` | `integer` | Não | ID do estado. |
| `address.country_id` | `integer` | Condicional | ID do país; obrigatório com `collaborator`. |
| `address.zipcode` | `string` | Não | CEP. |
| `address.address_one` | `string` | Não | Endereço linha 1. |
| `address.address_two` | `string` | Não | Endereço linha 2. |
| `address.address_three` | `string` | Não | Endereço linha 3. |
| `address.address_four` | `string` | Não | Endereço linha 4. |
| `address.addressable` | `string` | Não | Referência complementar. |
| `address.type` | `AddressTypeEnum` | Não | Tipo do endereço. |
| `contacts` | `array` | Condicional | Contatos do usuário; obrigatório com `collaborator`. |
| `contacts.*.type` | `ContactTypeEnum` | Sim | Tipo do contato. |
| `contacts.*.value` | `string` | Não | Valor do contato (ex.: e-mail). |
| `contacts.*.country_code` | `string` | Não | Código do país. |
| `contacts.*.number` | `string` | Não | Número do telefone. |
| `contacts.*.contactable` | `string` | Sim | Referência do contato. |

### Exemplo de Corpo

```json
{
  "collaborator": true,
  "device": "web",
  "name": "Usuário Exemplo",
  "gender": "male",
  "birth_date": "1990-01-01",
  "language": "pt-BR",
  "currency": "BRL",
  "email": "usuario@example.com",
  "password": "SenhaForte123",
  "password_confirmation": "SenhaForte123",
  "roles": ["guest"],
  "nationalities": ["BRA"],
  "address": {
    "city": "São Paulo",
    "state": "SP",
    "country": "Brasil",
    "country_id": 76,
    "zipcode": "00000-000",
    "address_one": "Rua Exemplo",
    "type": "residential"
  },
  "contacts": [
    {
      "type": "email",
      "value": "usuario@example.com",
      "contactable": "user"
    }
  ]
}
```

---

## Respostas

### Sucesso `201 Created`

```json
{
  "message": "Usuário Exemplo registrado com sucesso.",
  "token": "0000|exemploToken",
  "recently_created": true,
  "data": {
    "user": {
      "uuid": "00000000-0000-0000-0000-000000000000",
      "echo_uuid": "e000000000000000000000000000000000000",
      "name": "Usuário Exemplo",
      "email": "usuario@example.com",
      "avatar": {
        "url": "https://storage.example.com/avatar_thumbnail.webp",
        "usage": "avatar"
      },
      "language": "pt-BR",
      "roles": [
        {
          "id": 1,
          "platform": {
            "uuid": "11111111-2222-3333-4444-555555555555",
            "name": "Example Platform",
            "public_key": "aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee"
          },
          "name": "guest",
          "localized_name": "Convidado",
          "permissions": [
            {
              "subject": "complaint",
              "action": "store"
            }
          ]
        }
      ]
    }
  }
}
```

### Erro `400 Bad Request`

Retorna um objeto com mensagens de erro de validação.

---
