# Plataformas — Usuários (Backoffice)

## Endpoint

```
GET /backoffice/users
```

Lista os colaboradores vinculados às plataformas disponíveis para o operador backoffice.

---

## Código Fonte (branch `master`)

- [`BackofficePlatformUserController.php`](https://github.com/EchoSistema/monolith/blob/master/src/Domain/Shared/Http/Controllers/Backoffice/BackofficePlatformUserController.php) — método `index`.

---

## Autenticação

**Obrigatória** – Token Bearer com habilidade `backoffice`.

O usuário também precisa da permissão `index.all` para obter a listagem.

---

## Cabeçalhos

| Cabeçalho       | Tipo   | Obrigatório | Descrição |
| --------------- | ------ | ----------- | --------- |
| Authorization   | string | Sim         | Credencial `Bearer {token}`. |
| X-PUBLIC-KEY    | string | Sim         | Chave pública que identifica a plataforma solicitante. |
| Accept-Language | string | Não         | Locale IETF (ex.: `pt-BR`, `en`, `es`). Usado para localizar campos traduzíveis. |

---

## Parâmetros de consulta

| Parâmetro     | Tipo    | Obrigatório | Descrição | Padrão/Valores |
| ------------- | ------- | ----------- | --------- | -------------- |
| `per_page`    | integer | Não         | Número de itens por página na paginação. | `25` |
| `page`        | integer | Não         | Página atual quando paginando. | `1` |
| `no_paginate` | boolean | Não         | Quando `true`, desabilita paginação e retorna todos os registros. | `false` |

> Os parâmetros aceitam variantes em `snake_case`, `camelCase` e `kebab-case`.

---

## Exemplos

### Exemplo de requisição

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <chave-publica>" \
  -H "Accept-Language: pt-BR" \
  "https://sandbox.seu-dominio.com/backoffice/users?per_page=25"
```

### Exemplo de resposta (`200`)

```json
{
  "data": [
    {
      "uuid": "f8086ec1-04f7-4d8a-9643-f363f78c5a74",
      "name": "Maria Silva",
      "email": "maria.silva@example.com",
      "regional_information": {
        "country": "Brasil",
        "state": "SP",
        "city": "São Paulo"
      },
      "identities": [
        { "document_type": "CPF", "document_number": "123.456.789-10" }
      ],
      "telephone": {
        "number": "+55 11 99999-0000"
      },
      "avatar_image": {
        "full_url": "https://cdn.example.com/users/maria/avatar.webp"
      },
      "profile_image": null,
      "platform": {
        "uuid": "75f508e7-83ba-451c-9c2a-3df2aaf9db11",
        "name": "Echo Educação"
      },
      "platform_roles": [
        {
          "role_id": 2,
          "role_name": "Admin",
          "platform": {
            "uuid": "75f508e7-83ba-451c-9c2a-3df2aaf9db11",
            "name": "Echo Educação"
          }
        }
      ]
    }
  ]
}
```

> Quando `no_paginate=false`, o recurso retorna metadados padrão de paginação do Laravel (`links` e `meta`).

---

## Estrutura JSON Explicada

| Campo                      | Tipo      | Descrição |
| -------------------------- | --------- | --------- |
| `data[]`                   | array     | Lista de usuários retornados. |
| `data[].uuid`              | uuid      | Identificador único do usuário. |
| `data[].name`              | string    | Nome completo do usuário. |
| `data[].email`             | string    | E-mail principal. |
| `data[].regional_information` | object | Dados regionais vinculados (país, estado, cidade). |
| `data[].identities[]`      | array     | Documentos associados (tipo e número). |
| `data[].telephone`         | object    | Telefone principal quando existente. |
| `data[].avatar_image`      | object    | Imagem de avatar, com `full_url`. |
| `data[].profile_image`     | object    | Imagem de perfil alternativa, quando cadastrada. |
| `data[].platform`          | object    | Plataforma principal do usuário. |
| `data[].platform_roles[]`  | array     | Cargos do usuário e respectivas plataformas. |

---

## Status HTTP

- `200` – Sucesso.
- `401` – Token ausente ou inválido.
- `403` – Usuário autenticado sem permissão `index.all`.
- `429` – Limite de requisições excedido.
- `500` – Erro interno não tratado.

---

## Erros

### Exemplo 403 — Usuário sem `index.all`

```json
{
  "message": "You are not allowed to perform this action."
}
```

---

## Notas

- A coleção retorna os relacionamentos `regionalInformation`, `identities`, `telephone`, `avatarImage`, `profileImage`, `platform`, `platformRoles` e `platformRoles.platform` já carregados.
- Use `no_paginate=true` apenas para exports ou integrações controladas, evitando respostas muito grandes.

---

## Relacionados

- [Plataformas — Listagem e Detalhe](BackofficePlatformIndex.md)

---

## Changelog

- 2025-10-04: Documento inicial.

