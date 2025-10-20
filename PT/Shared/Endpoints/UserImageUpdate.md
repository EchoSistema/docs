# Compartilhado – Atualizar Minha Imagem

## Endpoint

`POST /api/v1/me/image`

## Autenticação

Requer um token Bearer válido.

## Cabeçalhos

| Cabeçalho | Tipo | Obrigatório | Descrição |
| --------- | ---- | ----------- | --------- |
| Authorization | `string` | Sim | `Bearer <token>`. |
| X-PUBLIC-KEY  | `string` | Sim | Chave pública da plataforma. |
| Accept-Language | `string` | Não | Idioma para mensagens de validação. |
| Content-Type | `string` | Sim | `multipart/form-data` para upload de arquivo ou `application/json` para URL/codificado. |

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
curl -X POST https://api.example.com/api/v1/me/image \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <platform-key>" \
  -F "name=Meu Avatar" \
  -F "usage=avatar" \
  -F "image_file=@/caminho/para/avatar.jpg"
```

## Exemplo de Solicitação (JSON com URL)

```json
{
  "name": "Meu Avatar",
  "usage": "avatar",
  "image_url": "https://exemplo.com/imagens/avatar.jpg"
}
```

## Exemplo de Solicitação (JSON com Base64)

```json
{
  "name": "Meu Avatar",
  "usage": "avatar",
  "image_encoded": "data:image/jpeg;base64,/9j/4AAQSkZJRgABAQEAYABgAAD..."
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
    "name": "Meu Avatar",
    "slug": "meu-avatar",
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

- 401 Não autorizado — token ausente/inválido
- 422 Entidade não processável — erros de validação
- 500 Erro interno do servidor

### Exemplo 422

```json
{
  "message": "Os dados fornecidos são inválidos.",
  "errors": {
    "name": ["O campo nome é obrigatório quando a URL da imagem não está presente."],
    "usage": ["O campo uso é obrigatório."],
    "image_file": ["O arquivo de imagem deve ser um arquivo do tipo: jpeg, png, jpg, gif, svg, webp, heic, heif."],
    "image_url": ["A URL da imagem deve ser uma URL válida."]
  }
}
```

## Notas

- Você deve fornecer **exatamente um** de: `image_file`, `image_url` ou `image_encoded`.
- O campo `usage` deve ser definido como `"avatar"` para avatares de usuário.
- As imagens enviadas são automaticamente otimizadas e convertidas para o formato WebP.
- A imagem de avatar anterior será substituída pela nova.

## Relacionado

- [Obter Perfil do Usuário](./UserProfile.md)
- [Atualizar Perfil do Usuário](./UserProfileUpdate.md)
- [Admin Atualizar Imagem do Usuário](./AdminUserImageUpdate.md)
