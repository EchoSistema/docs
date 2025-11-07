# IA – Usuários Admin (Detalhe)

## Endpoint

`GET /api/v1/ia/admin/users/{user:uuid}`

Retorna o perfil detalhado de um usuário da plataforma.

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

## Parâmetros

### Caminho

| Parâmetro | Tipo | Obrigatório | Descrição |
| --------- | ---- | ----------- | --------- |
| `user` | `uuid` | Sim | UUID do usuário. |

### Consulta

| Parâmetro | Tipo | Obrigatório | Descrição |
| --------- | ---- | ----------- | --------- |
| `role` | `boolean` | Não | Quando `true`, inclui os papéis (`roles`) do usuário na plataforma atual. |

---

## Exemplo de Resposta

```json
{
  "uuid": "9b732d6a-2e56-4db8-9b2e-6fd8c14e7f28",
  "name": "John Doe",
  "email": "john.doe@example.com",
  "avatar": "https://cdn.example.com/avatars/johndoe.webp",
  "roles": [
    {
      "id": 2,
      "uuid": "role-uuid",
      "name": "admin",
      "localized_name": "Administrador",
      "permissions": ["store.company"]
    }
  ],
  "telephone": "+55 11 99999-9999",
  "gender": "male",
  "birthday": "1990-05-15T00:00:00+00:00",
  "address": {
    "country": {"name": "Brasil", "code": "BR", "flag": "🇧🇷"},
    "state": "SP",
    "city": "São Paulo",
    "zipcode": "01000-000"
  },
  "nationalities": [
    {
      "uuid": "natio-uuid",
      "country": {"name": "Brasil", "code": "BR", "flag": "🇧🇷"}
    }
  ]
}
```

---

## Estrutura JSON Explicada

| Campo | Tipo | Descrição |
| ----- | ---- | --------- |
| `uuid` | `uuid` | Identificador único do usuário. |
| `name` | `string` | Nome completo. |
| `email` | `string` | E-mail. |
| `avatar` | `string` | URL do avatar quando disponível. |
| `roles[]` | `array` | Presente quando `role=true`; papéis do usuário na plataforma atual. |
| `telephone` | `string` | Telefone quando carregado. |
| `gender` | `string` | Gênero. |
| `birthday` | `string` | Data de nascimento ISO-8601. |
| `address` | `object` | Endereço quando carregado. |
| `nationalities[]` | `array` | Nacionalidades com país. |

---

## Status HTTP

- 200: Sucesso
- 401: Não autenticado
- 403: Proibido
- 404: Não encontrado

---

## Notas

- Use `role=true` para incluir os papéis e permissões do usuário na plataforma atual.
- Respeita cabeçalho `Accept-Language` para campos traduzíveis.

---

## Relacionados

- docs/PT/IA/Endpoints/PlatformUserIndex.md
- docs/PT/IA/Endpoints/PlatformUserStore.md
