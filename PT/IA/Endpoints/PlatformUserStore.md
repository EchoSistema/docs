# IA – Usuários Admin (Criar)

## Endpoint

`POST /api/v1/ia/admin/users`

Cria um novo usuário na plataforma (área administrativa de IA).

---

## Autenticação

Obrigatória – Token Bearer com habilidade `backoffice`.

---

## Cabeçalhos

| Cabeçalho | Tipo | Obrigatório | Descrição |
| --------- | ---- | ----------- | --------- |
| Authorization | `string` | Sim | `Bearer {token}`. |
| X-PUBLIC-KEY | `string` | Sim | Chave pública da plataforma. |
| Accept-Language | `string` | Recomendado | Idioma para campos traduzíveis (ex.: `pt-BR`, `en`, `es`). |

---

## Corpo da Requisição

Campos mínimos obrigatórios:

| Campo | Tipo | Obrigatório | Descrição |
| ----- | ---- | ----------- | --------- |
| `device` | `string` | Sim | Identificador do dispositivo/cliente. |
| `name` | `string` | Sim | Nome do usuário. |
| `email` | `string` | Sim | E-mail único na plataforma. |
| `password` | `string` | Sim | Senha (mín. 8). |
| `password_confirmation` | `string` | Sim | Confirmação da senha. |

Campos adicionais (quando `collaborator=true`, tornam-se exigidos em conjunto):

| Campo | Tipo | Obrigatório | Descrição |
| ----- | ---- | ----------- | --------- |
| `collaborator` | `boolean` | Não | Habilita fluxo de colaborador. |
| `gender` | `string` | Condicional | Gênero (enum). |
| `birth_date` | `date` | Condicional | Data de nascimento. |
| `language` | `string` | Não | Idioma do usuário (enum). |
| `currency` | `string` | Não | Moeda (código ISO suportado). |
| `roles[]` | `array<string>` | Condicional | Papéis (nomes válidos). |
| `nationalities[]` | `array` | Condicional | Nacionalidades. |
| `address` | `object` | Condicional | Endereço do usuário. |
| `contacts[]` | `array<object>` | Condicional | Contatos (e-mail/telefone/whatsapp, etc.). |

Observações de normalização:

- `contacts[*].type` aceita nomes ou `type`; é normalizado e `contactable` é definido como `user`.
- `address.addressable` é padrão `user`.

### Exemplo de Requisição

```json
{
  "device": "admin-panel",
  "name": "Jane Doe",
  "email": "jane@example.com",
  "password": "secret123",
  "password_confirmation": "secret123",
  "collaborator": true,
  "gender": "female",
  "birth_date": "1990-05-12",
  "roles": ["admin"],
  "address": {"country_id": 76},
  "contacts": [
    {"type": "public_email", "value": "jane.public@example.com"},
    {"type": "whatsapp", "country_code": "+55", "number": "11999999999"}
  ]
}
```

---

## Exemplo de Resposta

Observação: este endpoint não retorna `token` (uso interno/admin); inclui `recently_created` e `registered_by`.

```json
{
  "message": "Usuário Jane Doe criado com sucesso.",
  "recently_created": true,
  "data": {
    "user": {
      "uuid": "u1v2w3x4-y5z6-7890-1234-56789abcdef0",
      "echo_uuid": "echo-uuid",
      "name": "Jane Doe",
      "email": "jane@example.com",
      "avatar": null,
      "language": "pt-BR",
      "roles": [
        {
          "id": 2,
          "platform": {"uuid": "plat-uuid", "name": "Platform", "public_key": "..."},
          "name": "admin",
          "localized_name": "Administrador",
          "permissions": [{"subject": "company", "action": "store"}]
        }
      ],
      "registered_by": {"uuid": "admin-uuid", "name": "Admin"}
    }
  }
}
```

---

## Status HTTP

- 200: Sucesso (criado)
- 401: Não autenticado
- 403: Proibido
- 422: Erro de validação

---

## Notas

- Não retorna `token` por se tratar de criação via painel/admin.
- O corpo enviado é processado por um pipeline de registro que normaliza contatos e endereço.

---

## Relacionados

- docs/PT/IA/Endpoints/PlatformUserIndex.md
- docs/PT/IA/Endpoints/PlatformUserShow.md
