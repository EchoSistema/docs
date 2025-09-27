# ReputationBook – Perfil do usuário do Livro de Reclamações

## Consultar meu perfil

### Endpoint

```
GET /api/v1/complaint-book/me
```

Retorna dados do usuário autenticado, incluindo estatísticas e relacionamentos carregados.

### Autenticação

Obrigatória – `Bearer {token}` (`auth:sanctum`).

### Resposta

Segue o formato de `ComplaintBookUserResource`, com campos principais:

- Identificadores (`id`, `uuid`), nome, e-mail, gênero, data de nascimento.
- Contadores `complaints_count` e `reviews_count`.
- Contatos por tipo (`public_email`, `telephone`, `whatsapp`, ...), quando cadastrados.
- Endereços (coleção de `AddressResource`).
- Imagens carregadas (avatar, documentos, etc.).

### Status HTTP

- 200: Perfil retornado.
- 401: Token inválido/ausente.

---

## Enviar imagem para meu perfil

### Endpoint

```
POST /api/v1/complaint-book/me/image/upload
```

Permite anexar ou atualizar imagens (avatar, documentos, etc.) vinculadas ao usuário.

### Autenticação

Obrigatória – `Bearer {token}`.

### Cabeçalhos

| Cabeçalho | Tipo | Obrigatório | Descrição |
| --------- | ---- | ----------- | --------- |
| `Authorization` | string | Sim | Credencial `Bearer`. |
| `Content-Type` | string | Sim | `multipart/form-data` ou `application/json`, conforme carga. |

### Corpo aceito (`ImageStoreRequest`)

| Campo | Tipo | Obrigatório | Descrição |
| ----- | ---- | ----------- | --------- |
| `usage` | string | Sim | Finalidade da imagem (ex.: `avatar`). |
| `name` | string | Condicional | Nome amigável (obrigatório se `image_url` ausente). |
| `image_url` | string | Condicional | URL pública da imagem. |
| `image_encoded` | string | Condicional | Conteúdo Base64. |
| `image_file` | arquivo | Condicional | Upload binário (`jpeg`, `png`, `webp`, `heic`, ...). Máx. 2 MB. |
| `unique_id` | string | Não | Identificador para atualizar imagem existente. |
| `type` | string | Não | Tipo lógico (avatar, documento, etc.). |
| `raw` | array | Não | Metadados adicionais. |

> Ao enviar `image_url`, `image_encoded` ou `image_file`, os outros campos são opcionais; pelo menos um deve ser informado.

### Exemplo de resposta (201)

```json
{
  "data": {
    "uuid": "b9b16c30-0d6f-4d77-91f9-56a553d9f9a1",
    "unique_id": "avatar-main",
    "usage": "avatar",
    "url": "https://cdn.echosistema.com/avatars/user-123.png",
    "name": "Foto do perfil",
    "slug": "foto-do-perfil",
    "width": 512,
    "height": 512
  }
}
```

### Status HTTP

- 201: Imagem armazenada.
- 400/422: Arquivo inválido ou payload inconsistente.
- 401: Token inválido.

