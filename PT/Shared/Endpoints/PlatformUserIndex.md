# Plataforma – Documentação do Endpoint de Listagem de Usuários da Plataforma

## Endpoint

`GET /api/v1/reputation-book/users`
`GET /api/v1/ia/admin/users`

Recupera uma lista de usuários da plataforma com filtros por cargo, ocupação e área. Suporta critérios de busca detalhados e internacionalização.

---

## Autenticação

**Obrigatória** – Token Bearer com habilidade `backoffice`.

---

## Requisição

### Cabeçalhos Obrigatórios

| Cabeçalho          | Tipo     | Descrição |
| ------------------ | -------- | --------- |
| **Authorization**  | `string` | Credencial `Bearer {token}`. **Obrigatório**. |
| **X-PUBLIC-KEY**   | `string` | Chave pública que identifica a plataforma solicitante. **Obrigatório**. |
| **Accept-Language**| `string` | Código de idioma IETF (ex.: `pt-BR`, `en`, `es`). Determina a localização para campos traduzíveis. **Obrigatório**. |

### Parâmetros de Filtro

| Parâmetro                 | Tipo           | Obrigatório | Descrição |
| ------------------------- | -------------- | ----------- | --------- |
| `role`                    | `string/int`   | Não         | Identificador de cargo (ID numérico ou nome). |
| `roles[]`                 | `array`        | Não         | Lista de cargos (IDs numéricos ou nomes). |
| `role_id`                 | `integer`      | Não         | ID numérico do cargo. |
| `role_name`               | `string`       | Não         | Nome do cargo. |
| `role_ids[]`              | `array`        | Não         | Lista de IDs numéricos de cargos. |
| `role_names[]`            | `array`        | Não         | Lista de nomes de cargos. |
| `name`                    | `string`       | Não         | Nome do usuário (parcial ou completo). |
| `email`                   | `string`       | Não         | E-mail do usuário (correspondência exata, sem diferenciar maiúsculas). |
| `user_name`               | `string`       | Não         | Apelido para `name`. |
| `user_email`              | `string`       | Não         | Apelido para `email`. |
| `user_uuid`               | `uuid`         | Não         | UUID do usuário. |
| `job_occupation`          | `string/int`   | Não         | ID numérico, UUID ou título da ocupação. |
| `job_occupation_id`       | `string`       | Não         | ID numérico da ocupação. |
| `job_occupation_uuid`     | `string`       | Não         | UUID da ocupação. |
| `job_occupation_title`    | `string`       | Não         | Título da ocupação. |
| `occupation_area`         | `string/array` | Não         | Área de atuação como UUID, como string `conteudo:uso` ou array com `content` e `usage`. |
| `occupation_area.content` | `string`       | Não         | Nome ou palavra-chave da área de atuação. |
| `occupation_area.usage`   | `string`       | Não         | Tipo de uso da área de atuação. Permitido: `occupation_area_title`. |
| `occupation_area_id`      | `string`       | Não         | ID numérico da área de atuação. |
| `occupation_area_uuid`    | `string`       | Não         | UUID da área de atuação. |
| `has_job_occupation`      | `boolean`      | Não         | Filtra usuários pela presença (`true`) ou ausência (`false`) de ocupação. |
| `has_occupation_area`     | `boolean`      | Não         | Filtra usuários pela presença (`true`) ou ausência (`false`) de área de atuação. |
| `no_paginate`             | `boolean`      | Não         | Quando `true`, desabilita paginação e retorna todos os resultados. Padrão `false`. |
| `per_page`                | `integer`      | Não         | Itens por página na paginação. Padrão `25`. |

> **Nota:** Todos os parâmetros também aceitam variantes em camelCase (ex.: `roleId`, `userEmail`).

---

## Exemplo de Objeto de Resposta

```json
{
  "uuid": "9b732d6a-2e56-4db8-9b2e-6fd8c14e7f28",
  "name": "John Doe",
  "email": "john.doe@example.com",
  "image": "https://cdn.example.com/avatars/johndoe.webp",
  "gender": {
    "abbr": "M",
    "name": "Male"
  },
  "birth_date": "1990-05-15T00:00:00+00:00",
  "age": 34,
  "language": "en",
  "currency": {
    "id": "USD",
    "name": "US Dollar",
    "sign": "$"
  },
  "role": {
    "id": 2,
    "name": "Admin",
    "localized_name": "Administrator",
    "created_at": "2024-05-01T12:00:00+00:00"
  },
  "telephone": "+1-202-555-0147",
  "addresses": [
    "123 Example Street, District Name, Sample City, Sample State, Sample Country"
  ],
  "platform": {
    "user_status": "active",
    "name": "PlatformName"
  },
  "occupation": {
    "uuid": "bb4a7f72-39b6-4d23-a5c5-7186f1d2bcb3",
    "title": "Software Engineer",
    "is_default": true
  },
  "created_at": "2024-05-01T12:00:00+00:00",
  "updated_at": "2024-08-20T09:30:00+00:00"
}
```

---

## Explicação da Estrutura JSON

### `data[]` – Objeto Usuário

| Campo         | Tipo     | Descrição |
| ------------- | -------- | --------- |
| `uuid`        | `uuid`   | Identificador único do usuário. |
| `name`        | `string` | Nome completo do usuário. |
| `email`       | `string` | Endereço de e-mail do usuário. |
| `image`       | `string` | URL da imagem de avatar, se disponível. |
| `gender`      | `object` | Informações de gênero com abreviação e nome. |
| `birth_date`  | `string` | Data de nascimento em formato ISO-8601. |
| `age`         | `int`    | Idade calculada do usuário. |
| `language`    | `string` | Idioma preferido do usuário. |
| `currency`    | `object` | Informações de moeda com `id` (código ISO), `name` e `sign`. |
| `role`        | `object` | Cargo atribuído ao usuário na plataforma, com ID, nome, nome localizado e data de criação. |
| `telephone`   | `string` | Número de telefone do usuário, se disponível. |
| `addresses[]` | `array`  | Lista de endereços formatados vinculados ao usuário. |
| `platform`    | `object` | Dados específicos da plataforma, incluindo `user_status` e `name` (quando `platform` é consultado). |
| `occupation`  | `object` | Informações de ocupação se o usuário possuir ocupações carregadas. |
| `created_at`  | `string` | Data em que a atribuição de cargo foi criada (formato ISO-8601). |
| `updated_at`  | `string` | Data da última atualização dos dados do usuário (formato ISO-8601). |

### `gender`

| Campo | Tipo     | Descrição                         |
| ----- | -------- | --------------------------------- |
| `abbr`| `string` | Abreviação de gênero (ex.: `M`). |
| `name`| `string` | Nome completo do gênero.          |

### `currency`

| Campo | Tipo     | Descrição             |
| ----- | -------- | --------------------- |
| `id`  | `string` | Código ISO da moeda.  |
| `name`| `string` | Nome da moeda.        |
| `sign`| `string` | Símbolo da moeda.     |

### `role`

| Campo            | Tipo     | Descrição                                         |
| ---------------- | -------- | ------------------------------------------------- |
| `id`             | `int`    | ID numérico do cargo.                            |
| `name`           | `string` | Nome interno do cargo.                           |
| `localized_name` | `string` | Nome exibido localizado do cargo.                |
| `created_at`     | `string` | Data em que o cargo foi atribuído (ISO-8601).    |

### `occupation`

| Campo      | Tipo   | Descrição                                          |
| ---------- | ------ | -------------------------------------------------- |
| `uuid`     | `uuid` | UUID da experiência profissional.                 |
| `title`    | `string` | Título do cargo.                                 |
| `is_default` | `bool` | Indica se é a ocupação padrão do usuário.        |

---

## Notas

* Usuários só podem listar outros usuários com cargos inferiores aos seus.
* Se a consulta incluir `no_paginate=true`, os metadados de paginação são omitidos e todos os resultados são retornados.
* Filtros de string são **correspondências parciais sem diferenciação de maiúsculas**, salvo indicação contrária.
* Os parâmetros `role`, `roles`, `name` e `email` são mapeados internamente para `role_id`, `role_ids`, `user_name` e `user_email` antes da filtragem.
* Filtros de ocupação e área de atuação configuram automaticamente as flags `has_job_occupation` e `has_occupation_area`.
* Todos os parâmetros aceitam equivalentes em camelCase.
