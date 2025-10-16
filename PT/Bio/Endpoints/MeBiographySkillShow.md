# Bio – Minhas Habilidades de Biografia Mostrar

## Endpoint

```
GET /api/v1/bio/me/biography-skills/{uuid}
```

## Autenticação

Obrigatória – Bearer {token} com habilidade `backoffice` e permissão `domain:bio`.

## Cabeçalhos

| Cabeçalho           | Tipo   | Obrigatório | Descrição |
| ---------------- | ------ | -------- | ----------- |
| Authorization    | string | Yes      | `Bearer {token}`. |
| X-PUBLIC-KEY     | string | Yes      | Chave pública da plataforma. |

## Parâmetros

### Parâmetros de caminho

| Parâmetro | Tipo   | Obrigatório | Descrição |
| --------- | ------ | -------- | ----------- |
| uuid      | string | Yes      | UUID do recurso. |

## Status HTTP

- 200: Sucesso
- 401: Não autenticado
- 404: Não encontrado
- 500: Erro interno

## Relacionados

- [Minhas Habilidades de Biografia — Índice](MyBiographySkillsÍndice.md)
- [Minhas Habilidades de Biografia — Atualizar](MyBiographySkillsAtualizar.md)
