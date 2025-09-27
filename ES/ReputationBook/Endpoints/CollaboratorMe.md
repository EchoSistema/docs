# ReputationBook – Perfil del colaborador

## Endpoint

```
GET /api/v1/complaint-book/collaborators/me
```

Devuelve la información del colaborador autenticado en la plataforma, incluyendo los roles asignados.

## Autenticación

Obligatoria – `Bearer {token}` (`auth:sanctum`) con habilidades `backoffice` y `domain:reputationbook`.

## Respuesta

Sigue `CollaboratorResource`:

- Datos personales (`id`, `uuid`, `name`, `email`, `gender`, `birth_date`).
- `roles`: lista de roles asignados en la plataforma actual (nombre localizado, permisos agrupados por `subject`/`action`).
- `contacts`: mapa con contactos públicos (`public_email`, `telephone`, `whatsapp`, etc.).
- `platform`: información básica de la plataforma (id, name).
- `avatar` cuando existe y demás imágenes (`usage`, `url`, dimensiones).

## Códigos HTTP

- 200: Perfil retornado.
- 401: Token inválido.
- 403: El usuario no posee un rol para la plataforma (`auth.forbidden_platform`).

