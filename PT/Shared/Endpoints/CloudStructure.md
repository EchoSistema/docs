# Shared – Estrutura de Armazenamento em Nuvem

## Endpoint

```
GET /api/v1/cloud/structure?folder={folder}
```

## Descrição

Recupera a estrutura de pastas e arquivos do armazenamento em nuvem (S3) para um caminho de pasta específico. Retorna subpastas de primeiro nível e arquivos junto com seus caminhos completos. O endpoint valida que a pasta solicitada pertence à plataforma atual.

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
| folder    | string | Sim         | Caminho completo da pasta no S3. Deve conter o slug do domínio e nome da plataforma. Formato: `{tipo}/{domain-slug}/{platform-slug}` ou qualquer subpasta dentro. |

## Exemplos

### Exemplo de requisição (curl)

#### Pasta raiz da plataforma
```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <chave>" \
  "https://api.example.com/api/v1/cloud/structure?folder=files/real-estate/minha-plataforma"
```

#### Subpasta
```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <chave>" \
  "https://api.example.com/api/v1/cloud/structure?folder=files/real-estate/minha-plataforma/uploads"
```

### Exemplo de resposta - Sucesso (200)

```json
{
  "data": {
    "folders": [
      {
        "title": "uploads",
        "path": "files/real-estate/minha-plataforma/uploads",
        "file_count": 45,
        "total_size": 15728640,
        "total_size_formatted": "15.00 MB",
        "last_modified": "2025-10-21 14:30:25"
      },
      {
        "title": "documentos",
        "path": "files/real-estate/minha-plataforma/documentos",
        "file_count": 12,
        "total_size": 2097152,
        "total_size_formatted": "2.00 MB",
        "last_modified": "2025-10-20 09:15:10"
      },
      {
        "title": "relatorios",
        "path": "files/real-estate/minha-plataforma/relatorios",
        "file_count": 8,
        "total_size": 5242880,
        "total_size_formatted": "5.00 MB",
        "last_modified": "2025-10-19 16:45:00"
      }
    ],
    "files": [
      {
        "title": "leiame.txt",
        "path": "files/real-estate/minha-plataforma/leiame.txt",
        "size": 2048,
        "size_formatted": "2.00 KB",
        "mime_type": "text/plain",
        "extension": "txt",
        "last_modified": "2025-10-15 08:30:00",
        "last_modified_timestamp": 1729000200
      },
      {
        "title": "config.json",
        "path": "files/real-estate/minha-plataforma/config.json",
        "size": 1024,
        "size_formatted": "1.00 KB",
        "mime_type": "application/json",
        "extension": "json",
        "last_modified": "2025-10-18 11:20:45",
        "last_modified_timestamp": 1729254045
      }
    ],
    "base_path": "files/real-estate/minha-plataforma",
    "path": "files/real-estate/minha-plataforma"
  }
}
```

### Exemplo de resposta - Pasta vazia (200)

```json
{
  "data": {
    "folders": [],
    "files": [],
    "base_path": "files/real-estate/minha-plataforma/pasta-vazia",
    "path": "files/real-estate/minha-plataforma/pasta-vazia"
  }
}
```

## Estrutura JSON Explicada

### Objeto Response

| Campo              | Tipo     | Descrição |
| ------------------ | -------- | --------- |
| data               | object   | Objeto container para a estrutura de armazenamento |

### Objeto data

| Campo              | Tipo     | Descrição |
| ------------------ | -------- | --------- |
| folders            | array    | Lista de subpastas de primeiro nível no caminho solicitado |
| files              | array    | Lista de arquivos diretamente no caminho solicitado (não em subpastas) |
| base_path          | string   | O caminho da pasta solicitada (mesmo que o parâmetro path) |
| path               | string   | O caminho da pasta solicitada (mesmo que `base_path`, mantido para compatibilidade) |

### Objeto folders[]

| Campo                  | Tipo     | Descrição |
| ---------------------- | -------- | --------- |
| title                  | string   | Nome da pasta (último segmento do caminho) |
| path                   | string   | Caminho completo da pasta no armazenamento S3 |
| file_count             | integer  | Número total de arquivos na pasta (recursivo) |
| total_size             | integer  | Tamanho total de todos os arquivos em bytes |
| total_size_formatted   | string   | Tamanho total legível (ex: "15.00 MB") |
| last_modified          | string\|null | Data de última modificação do arquivo mais recente (formato Y-m-d H:i:s), null se não houver arquivos |

### Objeto files[]

| Campo                    | Tipo     | Descrição |
| ------------------------ | -------- | --------- |
| title                    | string   | Nome do arquivo com extensão |
| path                     | string   | Caminho completo do arquivo no armazenamento S3 |
| size                     | integer  | Tamanho do arquivo em bytes |
| size_formatted           | string   | Tamanho do arquivo legível (ex: "512.00 KB") |
| mime_type                | string   | Tipo MIME do arquivo (ex: "image/jpeg", "application/pdf") |
| extension                | string   | Extensão do arquivo sem ponto (ex: "jpg", "pdf") |
| last_modified            | string   | Data de última modificação (formato Y-m-d H:i:s) |
| last_modified_timestamp  | integer  | Timestamp Unix da última modificação |

## Status HTTP

- **200 OK**: Estrutura recuperada com sucesso
- **401 Unauthorized**: Token de autenticação ausente ou inválido
- **403 Forbidden**: Permissões insuficientes para acessar o armazenamento em nuvem
- **404 Not Found**: A pasta não pertence à plataforma atual ou não existe
- **500 Internal Server Error**: Erro ao acessar o armazenamento S3

## Erros

### Pasta não encontrada ou não autorizada (404)

Quando a pasta solicitada não contém o slug do domínio e nome da plataforma:

```json
{
  "message": "pasta não encontrada"
}
```

### Erro de acesso ao armazenamento (500)

```json
{
  "message": "Erro ao acessar o armazenamento em nuvem"
}
```

## Regras de Negócio

### 1. Validação de Plataforma
- O endpoint valida que o caminho da pasta contém `/{domain-slug}/{platform-slug}`
- Exemplo: Para a plataforma "Minha Plataforma" no domínio "real-estate", o caminho deve conter `/real-estate/minha-plataforma`
- Isso impede que usuários acessem pastas de outras plataformas
- Retorna 404 se a validação falhar

### 2. Formato do Caminho
- **Caminho completo obrigatório**: O parâmetro folder deve ser um caminho S3 completo
- **Exemplos válidos**:
  - `files/real-estate/minha-plataforma` (pasta raiz da plataforma)
  - `files/real-estate/minha-plataforma/uploads` (subpasta)
  - `images/real-estate/minha-plataforma/avatars/2024` (subpasta aninhada)
- **Exemplos inválidos**:
  - `minha-plataforma` (incompleto)
  - `files/outro-dominio/outra-plataforma` (plataforma diferente)

### 3. Listagem de Pastas
- Retorna apenas subdiretórios de primeiro nível do caminho solicitado
- Não faz recursão em pastas aninhadas
- Nomes de pastas são extraídos de caminhos de arquivos (pastas vazias não aparecem se não contiverem arquivos)

### 4. Filtragem de Arquivos
- Apenas arquivos diretamente no caminho solicitado são incluídos
- Arquivos em subdiretórios são excluídos
- Arquivos `.echo` são excluídos da listagem (uso interno do sistema)

### 5. Resultados Vazios
- Se não existirem pastas no caminho, o array `folders` estará vazio
- Se não existirem arquivos no caminho, o array `files` estará vazio
- Ambos os arrays podem estar vazios simultaneamente (resposta válida para pastas vazias)

### 6. Codificação de URL
- Caminhos de pastas com caracteres especiais devem ser codificados em URL
- Exemplo: `files/real-estate/minha-plataforma/minha pasta` se torna `files/real-estate/minha-plataforma/minha%20pasta`

## Segurança

### Isolamento de Plataforma
O endpoint impõe isolamento estrito de plataforma:
- Usuários só podem acessar pastas dentro do namespace de sua plataforma autenticada
- A validação verifica se o caminho da pasta contém o slug do domínio e nome da plataforma
- Tentativas de acessar pastas de outras plataformas resultarão em erros 404

### Prevenção de Path Traversal
- O mecanismo de validação previne ataques de path traversal
- Usuários não podem usar `..` ou outros truques para escapar da pasta de sua plataforma

## Considerações de Performance

- O endpoint lista todos os arquivos no caminho solicitado para determinar a estrutura
- O desempenho pode variar baseado no número de arquivos na pasta
- Pastas grandes com milhares de arquivos podem levar mais tempo para processar
- Os resultados não são armazenados em cache
- Considere paginação para pastas com muitos itens (não implementado atualmente)

## Endpoints Relacionados

- [CloudImages](CloudImages.md) - Obter estatísticas do diretório de imagens
- [CloudFiles](CloudFiles.md) - Obter estatísticas do diretório de arquivos

## Casos de Uso

### 1. Navegação em Navegador de Arquivos
Construir um navegador de arquivos hierárquico que permite aos usuários navegar por seu armazenamento em nuvem:

```javascript
// Navegar para raiz
GET /api/v1/cloud/structure?folder=files/real-estate/minha-plataforma

// Usuário clica na pasta "uploads"
GET /api/v1/cloud/structure?folder=files/real-estate/minha-plataforma/uploads

// Usuário clica na subpasta "2024"
GET /api/v1/cloud/structure?folder=files/real-estate/minha-plataforma/uploads/2024
```

### 2. Widget de Seleção de Pasta
Exibir pastas disponíveis para upload ou organização de arquivos:

```javascript
// Obter pastas disponíveis
GET /api/v1/cloud/structure?folder=files/real-estate/minha-plataforma

// Mostrar pastas ao usuário: "uploads", "documentos", "relatorios"
// Usuário seleciona "documentos" para upload de arquivo
```

### 3. Dashboard de Visão Geral de Armazenamento
Mostrar estrutura de pastas e conteúdo em um dashboard:

```javascript
// Obter estrutura raiz
GET /api/v1/cloud/structure?folder=files/real-estate/minha-plataforma

// Exibir:
// - Pastas: uploads (clique para expandir), documentos, relatorios
// - Arquivos: leiame.txt, config.json
```

### 4. Navegação Breadcrumb
Construir navegação breadcrumb para hierarquia de pastas:

```javascript
// Caminho atual: files/real-estate/minha-plataforma/uploads/2024/janeiro
// Breadcrumb: Home > uploads > 2024 > janeiro
// Cada segmento é clicável e chama o endpoint structure
GET /api/v1/cloud/structure?folder=files/real-estate/minha-plataforma              // Home
GET /api/v1/cloud/structure?folder=files/real-estate/minha-plataforma/uploads      // uploads
GET /api/v1/cloud/structure?folder=files/real-estate/minha-plataforma/uploads/2024 // 2024
GET /api/v1/cloud/structure?folder=files/real-estate/minha-plataforma/uploads/2024/janeiro // janeiro
```

## Notas de Implementação

### Detecção de Plataforma
O endpoint detecta automaticamente a plataforma atual do contexto do usuário autenticado usando `App::platform()`. A validação garante que a pasta solicitada pertence a esta plataforma.

### Extração de Nome de Pasta
Nomes de pastas são extraídos de caminhos de arquivos. Se uma pasta estiver completamente vazia (sem arquivos em qualquer profundidade), ela não aparecerá nos resultados.

### Componentes de Caminho
O caminho é dividido em componentes para extrair pastas de primeiro nível. Por exemplo:
- Caminho da requisição: `files/real-estate/minha-plataforma`
- Arquivo: `files/real-estate/minha-plataforma/uploads/documento.pdf`
- Pasta extraída: `uploads`

## Changelog

- **2025-10-21**: Endpoint criado com suporte para listagem de pastas e arquivos
- **2025-10-21**: Adicionada validação de plataforma para prevenir acesso entre plataformas
- **2025-10-21**: Parâmetro alterado de `type` para `folder` para suporte a caminho completo
- **2025-10-21**: Adicionado campo `base_path` à estrutura de resposta
- **2025-10-21**: Adicionadas mensagens de erro localizadas usando `validation.attributes.folder`
- **2025-10-21**: Adicionados metadados S3 para arquivos (size, mime_type, extension, last_modified)
- **2025-10-21**: Adicionados metadados S3 para pastas (file_count, total_size, last_modified)
