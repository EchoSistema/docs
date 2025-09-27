# ReputationBook – Gestão de reclamações (backoffice)

Todas as rotas abaixo exigem autenticação `Bearer {token}` (`auth:sanctum`) com habilidades `backoffice` e `domain:reputationbook`.

## Listar reclamações da plataforma

```
GET /api/v1/complaint-book/complaints
```

- Aceita os filtros de `ComplaintFilterRequest`.
- Retorna `ComplaintCollection` incluindo usuários, empresas e textos.
- Suporta `no_paginate=true` para consulta limitada (`limit`).

## Criar reclamação em nome de um usuário

```
POST /api/v1/complaint-book/complaints/{user}
```

| Parâmetro | Tipo | Descrição |
| --------- | ---- | --------- |
| `user` | string | ID ou UUID do usuário representado.

### Corpo

Mesmo payload de `POST /api/v1/complaints`, com os campos de `ComplaintStoreRequest`. O campo `created_by` é preenchido automaticamente com o colaborador autenticado.

### Resposta

`ComplaintResource` com relações básicas (`images`, `company`, `platform`).

## Exibir detalhes

```
GET /api/v1/complaint-book/complaints/{complaint}
```

Retorna a reclamação com relações estendidas (`images`, `statusChangeEvents`, `company` completa).

## Atualizar dados

```
PUT /api/v1/complaint-book/complaints/{complaint}
```

Mesmos campos de `ComplaintUpdateRequest`.

## Atualizar status

```
PATCH /api/v1/complaint-book/complaints/{complaint}/status
```

| Campo | Tipo | Obrigatório |
| ----- | ---- | ----------- |
| `status` | string | Sim (`ComplaintStatusEnum`). |

O usuário autenticado é registrado na linha do tempo (`statusChangeEvents`).

## Adicionar texto complementar

```
POST /api/v1/complaint-book/complaints/{complaint}/add-text
```

Segue `TextStoreRequest` (`content`, `language`, `usage`).

## Enviar imagem

```
POST /api/v1/complaint-book/complaints/{complaint}/images
```

Utiliza `ImageStoreRequest`. Defina `usage` (ex.: `evidence`) e forneça a imagem por URL, Base64 ou upload.

## Remover imagem específica

```
DELETE /api/v1/complaint-book/complaints/{complaint}/images/{uniqueId}
```

- Requer permissão (`destroy`, `delete`).
- Remove a imagem cujo identificador hexadecimal (`hex`) corresponde a `uniqueId`.
- Retorna `ComplaintResource` atualizado.

## Excluir reclamação

```
DELETE /api/v1/complaint-book/complaints/{complaint}
```

Remove a reclamação e retorna `204 No Content`.

## Status HTTP comuns

- 200: Operação concluída com retorno (`GET`, `PUT`, `PATCH`, `POST add-text/images`).
- 201: Imagens criadas.
- 204: Reclamação removida.
- 400/422: Validação falhou.
- 401/403: Falta de autenticação/permissões.
- 404: Reclamação ou imagem inexistente.

