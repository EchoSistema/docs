<!-- markdownlint-disable MD013 -->

# Authentication – Autenticar Usuário

## Endpoint

`POST /auth`

Realiza a autenticação de um usuário.

---

## Autenticação

Basic Auth. Envie o cabeçalho `Authorization: Basic <credenciais>`, onde `<credenciais>` é o resultado de `base64(usuario:senha)`.

---

## Requisição

### Cabeçalhos Obrigatórios

| Cabeçalho | Tipo | Descrição |
| --------- | ---- | --------- |
| **X-PUBLIC-KEY** | `string` | Chave pública da plataforma. |
| **Accept-Language** | `string` | Idioma da aplicação (ex.: `en`, `pt-BR`). Opcional. |
| **Authorization** | `string` | Credenciais em Basic Auth. |

### Corpo

| Campo | Tipo | Obrigatório | Descrição |
| ----- | ---- | ----------- | --------- |
| `device` | `string` | Sim | Identificador do dispositivo de acesso. |

---

## Exemplo de Requisição

```http
POST /auth HTTP/1.1
Authorization: Basic am9hbzpzZW5oYQ==
X-PUBLIC-KEY: 00000000-0000-0000-0000-000000000000
Accept-Language: pt-BR
Content-Type: application/json

{
  "device": "Mozilla/5.0 (Windows NT 10.0)"
}
```

---

## Exemplo de Resposta

```json
{
  "message": "Usuário autenticado com sucesso!",
  "token": "0001|tokenExemplo",
  "data": {
    "user": {
      "uuid": "00000000-0000-0000-0000-000000000000",
      "echo_uuid": "e1c1h1o1111111111111111111111111111",
      "name": "Usuário Exemplo",
      "email": "usuario.exemplo@dominio.com",
      "avatar": {
        "url": "https://example.com/avatar.png",
        "usage": "avatar"
      },
      "language": "pt",
      "roles": [
        {
          "id": 1,
          "platform": {
            "uuid": "22222222-2222-2222-2222-222222222222",
            "name": "Plataforma Exemplo",
            "public_key": "33333333-3333-3333-3333-333333333333"
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

---

## Estrutura JSON

| Campo | Tipo | Descrição |
| ----- | ---- | --------- |
| `message` | `string` | Mensagem de sucesso. |
| `token` | `string` | Token de acesso. |
| `data.user.uuid` | `uuid` | Identificador do usuário. |
| `data.user.echo_uuid` | `uuid` | Identificador interno. |
| `data.user.name` | `string` | Nome do usuário. |
| `data.user.email` | `string` | Email do usuário. |
| `data.user.avatar.url` | `string` | URL do avatar. |
| `data.user.avatar.usage` | `string` | Uso da imagem. |
| `data.user.language` | `string` | Idioma preferido. |
| `data.user.roles[]` | `array` | Perfis do usuário. |
| `data.user.roles[].name` | `string` | Nome do perfil. |
| `data.user.roles[].localized_name` | `string` | Nome localizado do perfil. |
| `data.user.roles[].permissions[]` | `array` | Permissões do perfil. |
| `data.user.roles[].permissions[].subject` | `string` | Domínio da permissão. |
| `data.user.roles[].permissions[].action` | `string` | Ação permitida. |

---

## Notas

* O token deve ser enviado em futuras requisições autenticadas.
* O campo `device` auxilia na identificação do equipamento de acesso.

<!-- markdownlint-enable MD013 -->
