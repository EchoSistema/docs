# ReputationBook – Perfil do colaborador

## Endpoint

```
GET /api/v1/complaint-book/collaborators/me
```

Retorna as informações do colaborador autenticado na plataforma, incluindo roles vinculados.

## Autenticação

Obrigatória – `Bearer {token}` (`auth:sanctum`) com habilidades `backoffice` e `domain:reputationbook`.

## Resposta

Estrutura conforme `CollaboratorResource`:

- Dados pessoais (`id`, `uuid`, `name`, `email`, `gender`, `birth_date`).
- `roles`: lista das roles atribuídas na plataforma atual (nome localizado, permissões agrupadas por `subject`/`action`).
- `contacts`: mapa com contatos públicos (`public_email`, `telephone`, `whatsapp`, etc.).
- `platform`: dados básicos da plataforma (id, name).
- `avatar` (quando cadastrado) e demais imagens (`usage`, `url`, dimensões).

## Status HTTP

- 200: Perfil retornado.
- 401: Token inválido.
- 403: Usuário não possui role na plataforma (`auth.forbidden_platform`).

