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

### `backoffice`
- **Objetivo:** canal global para eventos del panel administrativo.
- **Regla:** el usuario autenticado debe tener al menos un `Role` cuyo `can('backoffice.all')` devuelva `true`.
- **Sugerencia:** precarga la relación `roles` (por exemplo, `with('roles')`) en pruebas para evitar lecturas adicionales inesperadas.

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

## Integraciones de cliente

### Vue 3 + Laravel Echo
1. Instala las dependencias (`npm install laravel-echo pusher-js`) e inicializa Echo dentro de `resources/js/bootstrap.js` (o encapsúlalo como plugin) para reutilizar el token del usuario autenticado en toda la SPA.
2. Configura el plugin apuntando a las variables de entorno del Vite y al endpoint `/broadcasting/auth`, responsable de respetar las reglas descritas en este archivo:

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

3. Consuma Echo desde componentes de Vue 3 mediante la Composition API. Abandona el canal en `onBeforeUnmount` para evitar fugas de memoria:

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

- Para `users.{uuid}` usa `echo.private(`users.${userUuid}`)`. Para `platform.{publicKey}` aplica el mismo patrón y verifica que la `public_key` exista en la base.
- `Echo.private` agrega el prefijo `private-` automáticamente, por lo que `conversation.{uuid}` registrado aquí equivale a `private-conversation.{uuid}` en Pusher.

### Servicios Rust (workers e integraciones)
Los workers escritos en Rust pueden escuchar los mismos canales privados mediante WebSockets de Pusher. El ejemplo siguiente usa únicamente crates consolidados (`tokio`, [`tokio-websockets`](https://docs.rs/tokio-websockets/latest/tokio_websockets/), `reqwest`) y reutiliza el flujo de `/broadcasting/auth`.

1. Dependencias mínimas:

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

2. Ejemplo de suscripción al canal `conversation.{uuid}`:

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
    let backoffice_token = std::env::var("BACKOFFICE_TOKEN")?; // token autorizado para canales privados
    let channel = format!("private-conversation.{conversation_uuid}");
    let ws_url = format!(
        "wss://ws-{cluster}.pusher.com/app/{app_key}?protocol=7&client=rust&version=0.1&flash=false"
    );
    let uri: Uri = ws_url.parse()?;

    let (mut socket, _) = ClientBuilder::from_uri(uri).connect().await?;
    let first = socket
        .next()
        .await
        .ok_or_else(|| anyhow!("sin handshake de Pusher"))??;
    let payload = first
        .as_text()
        .ok_or_else(|| anyhow!("payload inicial no es texto"))?;
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
            println!("Evento recibido: {text}");
        }
    }

    Ok(())
}
```

- Cambia `channel` a `private-platform.{publicKey}` al consumir streams de plataforma.
- Para publicar eventos desde servicios Rust utiliza el SDK oficial `pusher-http-rust`, manteniendo toda la autorización centralizada en `routes/channels.php`.
- Guarda `BACKOFFICE_TOKEN`, app key y cluster en variables de entorno o en un gestor de secretos en lugar de hardcodearlos.

## Referencias
- [Laravel Broadcasting](https://laravel.com/docs/10.x/broadcasting)
- `routes/channels.php`
- `Domain\Shared\Models\Chat\Conversation`
- `Domain\Shared\Models\Platform\Platform`
