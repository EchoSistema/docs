# ReputationBook – Empresa reivindicada

## Consultar empresa (interno)

### Endpoint

```
GET /api/v1/complaint-book/claimed-company/{company}
```

Retorna detalhes completos da empresa reivindicada (inclui reclamações, documentos e mídias), respeitando as permissões do colaborador autenticado.

### Autenticação

Obrigatória – `Bearer {token}` com habilidades `backoffice` e `domain:reputationbook`.

### Parâmetros de caminho

| Parâmetro | Tipo | Descrição |
| --------- | ---- | --------- |
| `company` | string | ID, UUID ou slug da empresa. |

### Resposta

Estrutura baseada em `ClaimedCompanyResource`, contendo:

- Dados gerais (`uuid`, `name`, `assumed_name`, `rating`).
- Informações complementares (`addresses`, `contacts`, `social_medias`, `images`, `videos`).
- Documentos fiscais (`fiscal_documents`) quando `public=false`.
- Reclamações relacionadas (`complaints`).
- Dados do criador quando autenticado.

### Status HTTP

- 200: Empresa encontrada.
- 403: Plataforma fora de cobertura ou colaborador sem permissão.
- 404: Empresa não localizada / fora da área de cobertura.

---

## Consultar empresa (público)

### Endpoint

```
GET /api/v1/complaint-book/claimed-company/{company}/info
```

Retorna dados resumidos da empresa para exibição pública.

### Autenticação

Nenhuma. Middleware de plataforma permanece ativo (`X-PUBLIC-KEY`).

### Notas

- A resposta utiliza o mesmo recurso mas com flag `public=true`, omitindo documentos fiscais e reclamações internas.

---

## Verificar RUC

### Endpoint

```
GET /api/v1/complaint-book/claimed-company/ruc/{param}
```

Consulta o documento fiscal do tipo RUC. Quando encontrado, retorna os dados já cadastrados; caso contrário, tenta buscar via serviço externo.

### Parâmetros de caminho

`param` deve conter o RUC (números com ou sem DV). O controller normaliza o valor antes da busca.

### Resposta

- Existente: retorna `ClaimedCompanyResource` com `additional: { existent: true }`.
- Não existente: consulta o scraper (`http://ruc.local:3000`) e retorna `data` com os campos coletados e `existent: false`.

### Status HTTP

- 200: Resultado encontrado (cache ou API externa).
- 404: Sem dados para o RUC informado.
- 503: Serviço externo indisponível.

---

## Criar empresa

### Endpoint

```
POST /api/v1/complaint-book/claimed-company
```

Cria uma empresa associada à plataforma corrente.

### Autenticação

Obrigatória – `Bearer {token}` com habilidades `backoffice` e `domain:reputationbook`.

### Corpo (resumo de `CompanyStoreRequest`)

| Campo | Tipo | Obrigatório | Descrição |
| ----- | ---- | ----------- | --------- |
| `name` | string | Sim | Nome fantasia/razão social. |
| `assumed_name` | string | Não | Nome fantasia alternativo. |
| `raw.country` / `raw.state` / `raw.city` | string | Sim | Localização básica. |
| `raw.address.*` | array | Não | Componentes do endereço (logradouro, CEP, etc.). |
| `raw.email` | string | Condicional | E-mail de contato (obrigatório se `raw.phone` ausente). |
| `raw.phone.country_code`, `raw.phone.number` | string | Condicional | Telefone principal (obrigatório se `raw.email` ausente). |
| `raw.document.type` | string | Condicional | Tipo de documento fiscal (enum `FiscalDocumentTypeEnum`). |
| `raw.document.value` | string | Condicional | Valor do documento. |
| `contacts[]` | array | Não | Contatos adicionais (tipo + valor). |
| `social_medias[]` | array | Não | Lista de redes sociais. |
| `raw` | objeto | Não | Pode incluir `geolocation`, `website`, `google.place_id`, etc. |

### Resposta

Retorna `ClaimedCompanyResource` com dados completos.

### Status HTTP

- 201: Empresa criada.
- 400/422: Payload inválido ou incompleto.
- 401/403: Sem permissão.

---

## Atualizar empresa

### Endpoint

```
PUT /api/v1/complaint-book/claimed-company/{company}
```

Atualiza dados cadastrais, contatos ou endereço.

### Corpo (resumo de `CompanyUpdateRequest`)

| Campo | Tipo | Obrigatório | Descrição |
| ----- | ---- | ----------- | --------- |
| `type` | string | Sim | `company` (dados gerais) ou `address` (endereço). Preenchido automaticamente. |
| `name`, `assumed_name`, `email`, `website` | string | Condicional | Campos gerais (requeridos quando `type=company`). |
| `document.*` / `documents[]` | array | Não | Atualização de documentos fiscais. |
| `contacts[]` | array | Não | Atualização de contatos (tipo + `phone`/`country_code`). |
| `social_medias[]`, `videos[]` | array | Não | URLs de canais oficiais. |
| `address.*` | array | Condicional | Dados de endereço (obrigatórios quando `type=address`) incluindo país/estado/cidade IDs. |

### Status HTTP

- 200: Atualização aplicada.
- 400/422: Dados inválidos ou tipo não suportado.
- 404: Empresa inexistente.

---

## Enviar imagem da empresa

### Endpoint

```
POST /api/v1/complaint-book/claimed-company/{company}/images
```

Armazena ou substitui imagens da empresa (logos, materiais, etc.).

### Autenticação

Obrigatória – `Bearer {token}` com habilidades `backoffice` e `domain:reputationbook`.

### Corpo

Segue as mesmas regras de `ImageStoreRequest` (ver [perfil do usuário](ComplaintBookUsers.md)). Defina `usage` (`logo`, `banner`, ...) e forneça `image_url`, `image_encoded` ou `image_file`.

### Resposta

Retorna `ImageResource` com metadados da imagem armazenada.

### Status HTTP

- 201: Imagem salva.
- 400/422: Upload inválido.
- 404: Empresa inexistente.

