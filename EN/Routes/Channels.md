# Routes – Authorization Rules in `routes/channels.php`

This guide summarizes the authorization callbacks declared in `routes/channels.php` and how to extend them safely.

## Overview
- The file governs who can listen to Laravel Echo / Pusher private or presence channels.
- Each `Broadcast::channel` must return `true` to authorize access or `false` to deny it.
- Always validate against persisted data and relationships—never trust only client-provided parameters.

## Existing Channels

### `users.{uuid}`
- **Purpose:** ensure only the owner receives events on their direct user channel.
- **Rule:** compares the supplied `uuid` with the authenticated user using `hash_equals` to mitigate timing attacks.

### `conversation.{uuid}`
- **Purpose:** limit chat traffic to conversation participants.
- **Rule:** queries `Domain\Shared\Models\Chat\Conversation` by `uuid` and checks membership via the `participants` relationship.
- **Note:** keep the conversation UUID aligned between MySQL (`conversations`) and MongoDB (`messages`).

### `platform.{publicKey}`
- **Purpose:** enable platform-scoped channels.
- **Rule:** authorizes only when `Platform::where('public_key', $publicKey)` returns a record.

## Best Practices
- Add comments using `/** channel: description. */` before each `Broadcast::channel` block.
- Prefer chained queries (e.g., `return Conversation::where(...)->exists();`) to avoid extra branching.
- Use inner arrow functions (`static fn ($query) => ...`) for simple filters, while keeping the main closure as `function`.
- Cover critical authorization logic with feature tests whenever possible.

## Checklist for New Channels
1. Document the intent with the standard comment format.
2. Validate the authenticated user via existing models/relationships.
3. Return booleans only; avoid throwing exceptions inside the callback.
4. Update this documentation (EN/PT/ES) after adding the channel.
5. Review whether translated headers or responses need adjustments.

## References
- [Laravel Broadcasting](https://laravel.com/docs/10.x/broadcasting)
- `routes/channels.php`
- `Domain\Shared\Models\Chat\Conversation`
- `Domain\Shared\Models\Platform\Platform`
