# Compartilhado – Admin Atualizar Imagem do Usuário

## Endpoint

`POST /api/v1/users/{uuid}/image`

## Autenticação

Requer um token Bearer válido com habilidade `backoffice` e permissões apropriadas (`update.guest`, `update.collaborator` ou `update.all`).

## Cabeçalhos

| Cabeçalho | Tipo | Obrigatório | Descrição |
| --------- | ---- | ----------- | --------- |
| Authorization | `string` | Sim | `Bearer <token>` com habilidade backoffice. |
| X-PUBLIC-KEY  | `string` | Sim | Chave pública da plataforma. |
| Accept-Language | `string` | Não | Idioma para mensagens de validação. |
| Content-Type | `string` | Sim | `multipart/form-data` para upload de arquivo ou `application/json` para URL/codificado. |

## Parâmetros de URL

| Parâmetro | Tipo | Obrigatório | Descrição |
| --------- | ---- | ----------- | --------- |
| `uuid` | `uuid` | Sim | UUID do usuário cujo avatar será atualizado. |

## Corpo

A solicitação suporta três métodos diferentes para fornecer a imagem:

### Método 1: Upload de Arquivo (multipart/form-data)

| Campo | Tipo | Obrigatório | Descrição |
| ----- | ---- | ----------- | --------- |
| `name` | `string` | Sim | Nome da imagem (máx 255). |
| `usage` | `string` | Sim | Tipo de uso da imagem (ex., "avatar"). |
| `image_file` | `file` | Sim | Arquivo de imagem (jpeg, png, jpg, gif, svg, webp, heic, heif). Máx 2MB. |
| `type` | `string` | Não | Identificador do tipo de imagem (máx 255). |
| `raw` | `array` | Não | Metadados adicionais. |

### Método 2: URL da Imagem (application/json)

| Campo | Tipo | Obrigatório | Descrição |
| ----- | ---- | ----------- | --------- |
| `name` | `string` | Sim | Nome da imagem (máx 255). |
| `usage` | `string` | Sim | Tipo de uso da imagem (ex., "avatar"). |
| `image_url` | `url` | Sim | URL da imagem (máx 500). |
| `type` | `string` | Não | Identificador do tipo de imagem (máx 255). |
| `raw` | `array` | Não | Metadados adicionais. |

### Método 3: Codificado em Base64 (application/json)

| Campo | Tipo | Obrigatório | Descrição |
| ----- | ---- | ----------- | --------- |
| `name` | `string` | Sim | Nome da imagem (máx 255). |
| `usage` | `string` | Sim | Tipo de uso da imagem (ex., "avatar"). |
| `image_encoded` | `string` | Sim | Dados da imagem codificados em Base64. |
| `type` | `string` | Não | Identificador do tipo de imagem (máx 255). |
| `raw` | `array` | Não | Metadados adicionais. |

## Exemplo de Solicitação (Upload de Arquivo)

```bash
curl -X POST https://api.example.com/api/v1/users/550e8400-e29b-41d4-a716-446655440000/image \
  -H "Authorization: Bearer <admin-token>" \
  -H "X-PUBLIC-KEY: <platform-key>" \
  -F "name=Avatar do Usuário" \
  -F "usage=avatar" \
  -F "image_file=@/caminho/para/avatar.jpg"
```

## Exemplo de Solicitação (JSON com URL)

```json
{
  "name": "Avatar do Usuário",
  "usage": "avatar",
  "image_url": "https://exemplo.com/imagens/avatar-usuario.jpg"
}
```

## Sucesso `200 OK`

```json
{
  "data": {
    "uuid": "550e8400-e29b-41d4-a716-446655440000",
    "unique_id": "abc123def456",
    "usage": "avatar",
    "url": "https://cdn.exemplo.com/usuarios/joao/avatar-abc123def456.webp",
    "name": "Avatar do Usuário",
    "slug": "avatar-do-usuario",
    "width": 512,
    "height": 512,
    "creation_date": "2024-01-15T10:30:00+00:00"
  }
}
```

## Estrutura JSON

| Campo | Tipo | Descrição |
| ----- | ---- | --------- |
| `data.uuid` | `uuid` | Identificador da imagem. |
| `data.unique_id` | `string` | Identificador único hexadecimal. |
| `data.usage` | `string` | Tipo de uso da imagem. |
| `data.url` | `string` | URL completa para acessar a imagem. |
| `data.name` | `string` | Nome da imagem. |
| `data.slug` | `string` | Versão do nome compatível com URL. |
| `data.width` | `integer|null` | Largura da imagem em pixels. |
| `data.height` | `integer|null` | Altura da imagem em pixels. |
| `data.creation_date` | `datetime` | Quando a imagem foi criada. |

## Erros

- 401 Não autorizado — token ausente/inválido ou permissões insuficientes
- 403 Proibido — usuário não tem as permissões necessárias para atualizar este usuário
- 404 Não encontrado — usuário com UUID especificado não encontrado
- 422 Entidade não processável — erros de validação
- 500 Erro interno do servidor

### Exemplo 403

```json
{
  "message": "Você não tem permissão para executar esta ação."
}
```

### Exemplo 422

```json
{
  "message": "Os dados fornecidos são inválidos.",
  "errors": {
    "name": ["O campo nome é obrigatório quando a URL da imagem não está presente."],
    "usage": ["O campo uso é obrigatório."],
    "image_file": ["O arquivo de imagem deve ser um arquivo do tipo: jpeg, png, jpg, gif, svg, webp, heic, heif."]
  }
}
```

## Permissões

O usuário autenticado deve ter uma das seguintes permissões:
- `update.guest` — Pode atualizar usuários convidados
- `update.collaborator` — Pode atualizar usuários colaboradores
- `update.all` — Pode atualizar todos os usuários

## Notas

- Você deve fornecer **exatamente um** de: `image_file`, `image_url` ou `image_encoded`.
- O campo `usage` deve ser definido como `"avatar"` para avatares de usuário.
- As imagens enviadas são automaticamente otimizadas e convertidas para o formato WebP.
- A imagem de avatar anterior será substituída pela nova.
- Este endpoint requer a habilidade de token `backoffice`.

## Relacionado

- [Obter Perfil do Usuário](./UserProfile.md)
- [Admin Atualizar Usuário](./AdminUserUpdate.md)
- [Atualizar Minha Imagem](./UserImageUpdate.md)
