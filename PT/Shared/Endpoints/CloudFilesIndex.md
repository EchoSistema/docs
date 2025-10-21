# Shared – Estatísticas de Armazenamento de Arquivos (S3)

## Endpoint

```
GET /api/v1/cloud/files
```

## Autenticação

Obrigatória – Bearer {token}

## Cabeçalhos

| Cabeçalho     | Tipo | Obrigatório | Descrição |
| ---------------- | ------ | -------- | ----------- |
| Authorization    | string | Sim | Credencial `Bearer {token}`. |
| X-PUBLIC-KEY     | string | Sim      | Chave pública da plataforma. |
| Accept-Language  | string | Não       | Locale IETF (ex.: `pt-BR`, `en`, `es`). |

## Parâmetros

Nenhum parâmetro adicional é necessário. O endpoint retorna estatísticas do diretório de arquivos da plataforma atual no AWS S3.

## Exemplos

### Exemplo de requisição (curl)

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <key>" \
  -H "Accept-Language: pt-BR" \
  "https://sandbox.your-domain.com/api/v1/cloud/files"
```

### Exemplo de resposta

```json
{
  "data": {
    "path": "files/domain-slug/platform-name",
    "files": 28,
    "directories": 5,
    "total_size": 5242880,
    "total_size_formatted": "5 MB",
    "timestamp": "2025-10-21 10:30:45"
  }
}
```

## Estrutura JSON Explicada

| Campo | Tipo | Descrição |
| ----------- | ------- | ----------- |
| data        | object  | Estatísticas do diretório de arquivos |
| data.path   | string  | Caminho relativo no bucket S3 (formato: `files/{domain-slug}/{platform-name-slug}`) |
| data.files  | integer | Número total de arquivos (excluindo .echo) |
| data.directories | integer | Número total de subdiretórios |
| data.total_size | integer | Tamanho total em bytes |
| data.total_size_formatted | string | Tamanho formatado em formato legível (KB, MB, GB, etc) |
| data.timestamp | string | Data e hora da consulta (formato: Y-m-d H:i:s) |

## Status HTTP

- 200: Sucesso
- 401: Não autenticado
- 403: Proibido
- 500: Erro interno (possível falha de conexão com S3)

## Erros

```json
{
  "message": "Mensagem de erro"
}
```

Possíveis erros específicos:
- Credenciais S3 inválidas ou expiradas
- Permissões insuficientes no bucket S3
- Timeout de conexão com AWS S3

## Notas

- O endpoint cria automaticamente o arquivo `.echo` no S3 se não existir
- A cada consulta, uma nova linha é adicionada ao arquivo `.echo` com timestamp e estatísticas
- O arquivo `.echo` é excluído da contagem de arquivos e tamanho
- A estrutura de pastas no S3 é baseada no slug do domínio e nome da plataforma atual
- Path format: `files/{domain-slug}/{platform-name-slug}/`
- O cálculo é feito recursivamente incluindo todos os subdiretórios

## Relacionados

- [Estatísticas de Imagens](CloudImagesIndex.md)

## Changelog

- 2025-10-21: Documentação inicial
