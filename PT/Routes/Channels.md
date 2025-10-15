# Routes – Autorização de `routes/channels.php`

Este guia descreve as regras de autorização definidas em `routes/channels.php` e como evoluí-las com segurança.

## Visão Geral
- O arquivo controla quem pode ouvir canais privados/presence usados pelo Laravel Echo ou Pusher.
- Cada `Broadcast::channel` deve retornar `true` para autorizar ou `false` para negar o acesso.
- Valide sempre contra dados persistidos e relacionamentos; nunca confie apenas em parâmetros enviados pelo cliente.

## Canais Existentes

### `users.{uuid}`
- **Objetivo:** permitir que apenas o próprio usuário receba eventos direcionados ao seu UUID.
- **Regra:** compara o `uuid` fornecido com o do usuário autenticado usando `hash_equals` para mitigar *timing attacks*.

### `conversation.{uuid}`
- **Objetivo:** restringir mensagens de chat aos participantes da conversa.
- **Regra:** consulta `Domain\Shared\Models\Chat\Conversation` filtrando pelo `uuid` e verifica a existência do usuário na relação `participants`.
- **Observação:** mantenha o `uuid` consistente entre MySQL (`conversations`) e MongoDB (`messages`).

### `platform.{publicKey}`
- **Objetivo:** liberar canais específicos por plataforma.
- **Regra:** autoriza somente se existir `Platform::where('public_key', $publicKey)` correspondente.

## Boas Práticas
- Use comentários no formato `/** canal: descrição. */` antes de cada `Broadcast::channel`.
- Prefira encadear consultas (`return Conversation::where(...)->exists();`) para reduzir código extra.
- Utilize arrow functions internas (`static fn ($query) => ...`) para filtros simples; mantenha a closure principal com `function`.
- Adicione testes de feature cobrindo autorizações críticas quando possível.

## Checklist para Novos Canais
1. Documentar o propósito do canal com comentário padrão.
2. Validar o usuário autenticado usando modelos/relações existentes.
3. Retornar apenas booleano; evitar exceções dentro da callback.
4. Atualizar esta documentação (PT/EN/ES) após criar o canal.
5. Revisar se cabeçalhos ou respostas traduzidas precisam ser ajustados.

## Referências
- [Laravel Broadcasting](https://laravel.com/docs/10.x/broadcasting)
- `routes/channels.php`
- `Domain\Shared\Models\Chat\Conversation`
- `Domain\Shared\Models\Platform\Platform`
