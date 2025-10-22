# Shared – Estrutura de Pastas e Arquivos (Cloud)

## Endpoint

```
GET /api/v1/platform/cloud/structure/{type}
```

## Autenticação

Obrigatória – Bearer {token} com habilidade adequada

## Cabeçalhos

| Cabeçalho          | Tipo     | Obrigatório | Descrição |
| ------------------ | -------- | ----------- | --------- |
| Authorization      | string   | Sim         | Credencial `Bearer {token}`. |
| X-PUBLIC-KEY       | string   | Sim         | Chave pública da plataforma. |

## Parâmetros

### Parâmetros de caminho

| Parâmetro | Tipo   | Obrigatório | Descrição |
| --------- | ------ | ----------- | --------- |
| type      | string | Sim         | Tipo de armazenamento. Valores aceitos: `images`, `files` |

## Exemplos

### Exemplo de requisição (curl)

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <chave>" \
  "https://sandbox.seu-dominio.com/api/v1/platform/cloud/structure/files"
```

### Exemplo de resposta

```json
{
  "data": {
    "folders": [
      {
        "title": "uploads",
        "path": "files/domain-slug/platform-slug/uploads"
      },
      {
        "title": "documents",
        "path": "files/domain-slug/platform-slug/documents"
      }
    ],
    "files": [
      {
        "title": "readme.txt",
        "path": "files/domain-slug/platform-slug/readme.txt"
      },
      {
        "title": "config.json",
        "path": "files/domain-slug/platform-slug/config.json"
      }
    ],
    "path": "files/domain-slug/platform-slug"
  }
}
```

## Estrutura JSON Explicada

| Campo          | Tipo     | Descrição |
| -------------- | -------- | --------- |
| data           | object   | Objeto contendo a estrutura de pastas e arquivos |
| data.folders   | array    | Lista de pastas de primeiro nível |
| data.folders[].title | string | Nome da pasta |
| data.folders[].path  | string | Caminho completo da pasta no S3 |
| data.files     | array    | Lista de arquivos diretos (não em subpastas) |
| data.files[].title | string | Nome do arquivo |
| data.files[].path  | string | Caminho completo do arquivo no S3 |
| data.base_path | string   | Caminho base do armazenamento |
| data.path      | string   | Caminho base do armazenamento (duplicado para compatibilidade) |

## Status HTTP

- 200: Sucesso
- 401: Não autenticado
- 403: Proibido
- 500: Erro interno

## Erros

Erros padrão do sistema:

```json
{
  "message": "Erro ao acessar o armazenamento"
}
```

## Notas

- O parâmetro `type` define o tipo de armazenamento a ser listado
- Valores aceitos para `type`: `images` (imagens) ou `files` (arquivos)
- O endpoint retorna apenas pastas e arquivos de primeiro nível
- Arquivos `.echo` são excluídos da listagem (uso interno do sistema)
- O caminho base é construído automaticamente baseado na plataforma atual
- Formato do path: `{type}/{domain-slug}/{platform-slug}`
- Não retorna conteúdo de subpastas recursivamente

## Relacionados

- [CloudImages](CloudImages.md) - Estatísticas de imagens
- [CloudFiles](CloudFiles.md) - Estatísticas de arquivos

## Changelog

- 2025-10-21: Endpoint criado com suporte a listagem de pastas e arquivos
