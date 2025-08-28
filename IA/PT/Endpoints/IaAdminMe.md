<!-- markdownlint-disable MD013 -->

# IA – Perfil do Administrador

## Endpoint

`GET /api/v1/ia/admin/me`

Retorna o perfil do usuário autenticado para a plataforma atual.

---

## Autenticação

Token Bearer via Sanctum.

---

## Requisição

### Cabeçalhos Obrigatórios

| Cabeçalho | Tipo | Descrição |
| --------- | ---- | --------- |
| **Authorization** | `string` | Credencial Bearer {token}. |
| **X-PUBLIC-KEY** | `string` | Chave pública que identifica a plataforma. |
| **Accept-Language** | `string` | Idioma para campos traduzíveis. Opcional. |

Nenhum parâmetro de consulta é suportado.

---

## Exemplo de Resposta

```json
{
  "id": 123,
  "uuid": "u1v2w3x4-y5z6-7890-1234-56789abcdef0",
  "name": "Jane Doe",
  "gender": "female",
  "birth_date": "1990-05-12",
  "email": "jane@example.com",
  "roles": ["admin"],
  "permissions": ["store.company"],
  "contacts": {
    "public_email": {
      "email": "jane.public@example.com",
      "type": "public_email"
    },
    "whatsapp": {
      "ddi": "+55",
      "ddd": "11",
      "number": "999999999",
      "full": "+55 11 99999-9999",
      "type": "whatsapp"
    }
  },
  "platform": {"id": 1, "name": "PlatformName"},
  "images": {
    "avatar": {
      "uuid": "img-uuid",
      "unique_id": "a1b2c3",
      "usage": "avatar",
      "url": "https://cdn.example.com/avatar.jpg",
      "name": "Avatar",
      "slug": "avatar",
      "width": 512,
      "height": 512,
      "creation_date": "2025-01-01T12:00:00Z"
    }
  }
}
```

---

## Estrutura JSON

| Campo | Tipo | Descrição |
| ----- | ---- | --------- |
| `id` | `integer` | ID do usuário. |
| `uuid` | `uuid` | Identificador do usuário. |
| `name` | `string` | Nome completo. |
| `gender` | `string` | Gênero. |
| `birth_date` | `date` | Data de nascimento. |
| `email` | `string` | Email de contato. |
| `roles[]` | `array` | Perfis atribuídos na plataforma. |
| `permissions[]` | `array` | Permissões concedidas. |
| `contacts` | `object` | Contatos públicos organizados por tipo. |
| `platform` | `object` | Resumo da plataforma (id, nome). |
| `images` | `object` | Imagens enviadas organizadas por uso. |

---

## Notas

* `401 Unauthorized` para token ausente ou inválido.
* `403 Forbidden` se o usuário não possuir papel diferente de convidado na plataforma.

<!-- markdownlint-enable MD013 -->
