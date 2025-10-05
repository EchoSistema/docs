# Shared – Listar Áreas de Domínio (Backoffice)

## Endpoint

```
GET /api/v1/backoffice/domain-areas
```

## Autenticação

Obrigatória – Bearer {token} com permissão `index.all`

## Cabeçalhos

| Cabeçalho          | Tipo     | Obrigatório | Descrição |
| ------------------ | -------- | ----------- | --------- |
| Authorization      | string   | Sim         | Credencial `Bearer {token}`. |
| X-PUBLIC-KEY       | string   | Sim         | Chave pública da plataforma. |

## Parâmetros

Este endpoint não aceita parâmetros de caminho ou consulta.

## Exemplos

### Exemplo de requisição (curl)

```bash
curl -X GET \
  -H "Authorization: Bearer <token>" \
  -H "X-PUBLIC-KEY: <chave>" \
  "https://sandbox.seu-dominio.com/api/v1/backoffice/domain-areas"
```

### Exemplo de resposta

```json
{
  "data": [
    {
      "uuid": "550e8400-e29b-41d4-a716-446655440000",
      "name": "Biografia",
      "slug": "bio",
      "is_active": true
    },
    {
      "uuid": "660e8400-e29b-41d4-a716-446655440001",
      "name": "Saúde",
      "slug": "healthcare",
      "is_active": true
    }
  ]
}
```

## Estrutura JSON Explicada

| Campo           | Tipo     | Descrição |
| --------------- | -------- | --------- |
| data[]          | array    | Lista de áreas de domínio disponíveis no sistema |
| data[].uuid     | string   | Identificador único universal da área de domínio |
| data[].name     | string   | Nome da área de domínio |
| data[].slug     | string   | Slug da área de domínio (identificador textual único) |
| data[].is_active| boolean  | Indica se a área de domínio está ativa no sistema |

## Status HTTP

- 200: Sucesso
- 401: Não autenticado
- 403: Proibido (sem permissão `index.all`)
- 500: Erro interno

## Erros

### 403 Forbidden
Usuário não possui a permissão necessária:

```json
{
  "message": "Forbidden"
}
```

### 401 Unauthorized
Token ausente ou inválido:

```json
{
  "message": "Unauthenticated."
}
```

## Notas

- Este endpoint retorna todas as áreas de domínio cadastradas no sistema, incluindo as inativas
- As áreas de domínio são usadas para organizar plataformas, categorias e tipos de itens
- O campo `slug` pode ser usado para identificar áreas de domínio específicas programaticamente
- A verificação de permissão é feita através do método `checkPermissions('index.all')` do controller
- Apenas os campos `uuid`, `name`, `slug` e `is_active` são retornados (campos selecionados explicitamente)

## Relacionados

- [Listar Plataformas](./BackofficePlatformIndex.md)
- [Listar Categorias por Domínio](./CategoryGroupIndex.md)

## Changelog

- 2025-10-05: Documentação inicial criada
