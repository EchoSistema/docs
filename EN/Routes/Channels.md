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

### `backoffice`
- **Purpose:** global stream for backoffice dashboards.
- **Rule:** requires the authenticated user to have at least one `Role` whose `can('backoffice.all')` returns `true`.
- **Tip:** eager-load the `roles` relationship (e.g., `with('roles')`) when testing to avoid unexpected additional queries.

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

## Client Integrations

### Vue 3 + Laravel Echo
1. Install the dependencies (`npm install laravel-echo pusher-js`) and bootstrap Echo inside `resources/js/bootstrap.js` (or wrap it as a plugin) so the logged user token can be reused globally.
2. Configure the plugin pointing to the Vite env variables and `/broadcasting/auth`, which enforces the rules defined in this document:

```ts
// resources/js/plugins/echo.ts
import Echo from 'laravel-echo';
import Pusher from 'pusher-js';

window.Pusher = Pusher;

const token = window.localStorage.getItem('backoffice_token');

export const echo = new Echo({
    broadcaster: 'pusher',
    key: import.meta.env.VITE_PUSHER_APP_KEY,
    cluster: import.meta.env.VITE_PUSHER_APP_CLUSTER ?? 'mt1',
    wsHost: import.meta.env.VITE_PUSHER_HOST ?? `ws-${import.meta.env.VITE_PUSHER_APP_CLUSTER}.pusher.com`,
    wsPort: Number(import.meta.env.VITE_PUSHER_PORT ?? 80),
    wssPort: Number(import.meta.env.VITE_PUSHER_PORT ?? 443),
    forceTLS: (import.meta.env.VITE_PUSHER_SCHEME ?? 'https') === 'https',
    enabledTransports: ['ws', 'wss'],
    authEndpoint: '/broadcasting/auth',
    auth: {
        headers: {
            Authorization: token ? `Bearer ${token}` : undefined,
        },
    },
});
```

3. Consume Echo from Vue 3 components through the Composition API. Always leave the channel during `onBeforeUnmount` to avoid memory leaks:

```ts
// useConversationChannel.ts
import { onBeforeUnmount, onMounted } from 'vue';
import { echo } from '@/plugins/echo';

export function useConversationChannel(uuid: string, onMessage: (payload: any) => void) {
    onMounted(() => {
        echo
            .private(`conversation.${uuid}`)
            .listen('.message.sent', (event: any) => onMessage(event));
    });

    onBeforeUnmount(() => {
        echo.leave(`conversation.${uuid}`);
    });
}
```

- For `users.{uuid}` call `echo.private(`users.${userUuid}`)`. For `platform.{publicKey}` follow the same pattern and ensure the `public_key` exists in the database.
- `Echo.private` automatically prefixes the channel with `private-`, therefore `conversation.{uuid}` defined here maps to `private-conversation.{uuid}` on Pusher.

### Rust services (workers & integrations)
Rust workers can listen to the same private channels through Pusher WebSockets. The example below sticks to mainstream crates (`tokio`, [`tokio-websockets`](https://docs.rs/tokio-websockets/latest/tokio_websockets/), `reqwest`) and reuses the `/broadcasting/auth` flow.

1. Minimal dependencies:

```toml
# Cargo.toml
anyhow = "1"
futures-util = "0.3"
http = "1"
reqwest = { version = "0.11", features = ["json", "rustls-tls"] }
serde = { version = "1.0", features = ["derive"] }
serde_json = "1.0"
tokio = { version = "1", features = ["macros", "rt-multi-thread"] }
tokio-websockets = { version = "0.13", default-features = false, features = ["client", "rustls-native-roots"] }
```

2. Sample subscription for `conversation.{uuid}`:

```rust
use anyhow::{anyhow, Result};
use futures_util::{SinkExt, StreamExt};
use http::Uri;
use reqwest::Client;
use serde::Deserialize;
use serde_json::json;
use tokio_websockets::{ClientBuilder, Message};

#[derive(Deserialize)]
struct ConnectionEnvelope {
    event: String,
    data: String,
}

#[derive(Deserialize)]
struct ConnectionData {
    socket_id: String,
    activity_timeout: u32,
}

#[derive(Deserialize)]
struct AuthResponse {
    auth: String,
}

#[tokio::main]
async fn main() -> Result<()> {
    let app_key = std::env::var("PUSHER_APP_KEY")?;
    let cluster = std::env::var("PUSHER_APP_CLUSTER").unwrap_or_else(|_| "mt1".into());
    let conversation_uuid = std::env::var("CONVERSATION_UUID")?;
    let backoffice_token = std::env::var("BACKOFFICE_TOKEN")?; // token allowed to join the private channels
    let channel = format!("private-conversation.{conversation_uuid}");
    let ws_url = format!(
        "wss://ws-{cluster}.pusher.com/app/{app_key}?protocol=7&client=rust&version=0.1&flash=false"
    );
    let uri: Uri = ws_url.parse()?;

    let (mut socket, _) = ClientBuilder::from_uri(uri).connect().await?;
    let first = socket
        .next()
        .await
        .ok_or_else(|| anyhow!("missing Pusher handshake"))??;
    let payload = first
        .as_text()
        .ok_or_else(|| anyhow!("handshake payload is not text"))?;
    let envelope: ConnectionEnvelope = serde_json::from_str(payload)?;
    let connection: ConnectionData = serde_json::from_str(&envelope.data)?;

    let auth = Client::new()
        .post("https://backoffice.echosistema.app/broadcasting/auth")
        .bearer_auth(backoffice_token)
        .form(&[("channel_name", channel.clone()), ("socket_id", connection.socket_id.clone())])
        .send()
        .await?
        .json::<AuthResponse>()
        .await?
        .auth;

    let subscribe = json!({
        "event": "pusher:subscribe",
        "data": { "channel": channel, "auth": auth }
    });
    socket.send(Message::text(subscribe.to_string())).await?;

    while let Some(message) = socket.next().await {
        let event = message?;
        if let Some(text) = event.as_text() {
            println!("Received event: {text}");
        }
    }

    Ok(())
}
```

- Switch `channel` to `private-platform.{publicKey}` when consuming the platform stream.
- Use the official `pusher-http-rust` SDK to publish events from Rust services, but keep the authorization logic centralized in `routes/channels.php`.
- Store `BACKOFFICE_TOKEN`, app key, and cluster in env vars or a secret manager rather than hardcoding them in binaries.

## References
- [Laravel Broadcasting](https://laravel.com/docs/10.x/broadcasting)
- `routes/channels.php`
- `Domain\Shared\Models\Chat\Conversation`
- `Domain\Shared\Models\Platform\Platform`
