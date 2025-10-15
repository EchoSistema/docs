# Routes – Reglas de autorización en `routes/channels.php`

Esta guía resume las callbacks de autorización definidas en `routes/channels.php` y cómo extenderlas de forma segura.

## Visión general
- El archivo controla quién puede escuchar canales privados/presence usados por Laravel Echo o Pusher.
- Cada `Broadcast::channel` debe devolver `true` para autorizar el acceso o `false` para negarlo.
- Valida siempre contra datos persistidos y relaciones; nunca confíes únicamente en parámetros enviados por el cliente.

## Canales existentes

### `users.{uuid}`
- **Objetivo:** garantizar que solo el propio usuario reciba eventos en su canal directo.
- **Regla:** compara el `uuid` recibido con el del usuario autenticado usando `hash_equals` para mitigar *timing attacks*.

### `conversation.{uuid}`
- **Objetivo:** limitar el chat a los participantes de la conversación.
- **Regla:** consulta `Domain\Shared\Models\Chat\Conversation` por `uuid` y verifica la pertenencia mediante la relación `participants`.
- **Nota:** mantén alineado el UUID de la conversación entre MySQL (`conversations`) y MongoDB (`messages`).

### `platform.{publicKey}`
- **Objetivo:** habilitar canales específicos por plataforma.
- **Regla:** autoriza solo si existe un registro en `Platform::where('public_key', $publicKey)`.

## Buenas prácticas
- Añade comentarios en el formato `/** canal: descripción. */` antes de cada bloque `Broadcast::channel`.
- Prefiere consultas encadenadas (`return Conversation::where(...)->exists();`) para evitar ramificaciones extra.
- Usa funciones flecha internas (`static fn ($query) => ...`) para filtros simples y mantén la closure principal con `function`.
- Cubre la lógica crítica de autorización con pruebas de feature cuando sea posible.

## Checklist para nuevos canales
1. Documentar el propósito con el comentario estándar.
2. Validar al usuario autenticado mediante modelos/relaciones existentes.
3. Devolver únicamente valores booleanos; evita lanzar excepciones dentro de la callback.
4. Actualizar esta documentación (ES/PT/EN) después de agregar el canal.
5. Revisar si es necesario ajustar encabezados o respuestas traducidas.

## Referencias
- [Laravel Broadcasting](https://laravel.com/docs/10.x/broadcasting)
- `routes/channels.php`
- `Domain\Shared\Models\Chat\Conversation`
- `Domain\Shared\Models\Platform\Platform`
