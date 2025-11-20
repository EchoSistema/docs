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

### `backoffice`
- **Objetivo:** canal global para eventos do painel administrativo.
- **Regra:** o usuário precisa possuir pelo menos uma `Role` cujo método `can('backoffice.all')` retorne `true`.
- **Dica:** o teste deve carregar as relações de `roles` para evitar consultas extras em produção.

### `conversation.{uuid}`
- **Objetivo:** restringir mensagens de chat aos participantes da conversa.
- **Regra:** busca `Conversation::where('uuid', $uuid)` e usa `whereHas('participants', fn ($query) => $query->where('user_id', $user->id))` antes de autorizar.
- **Observação:** mantenha o `uuid` consistente entre MySQL (`conversations`) e MongoDB (`messages`).

### `platform.{publicKey}`
- **Objetivo:** liberar canais específicos por plataforma (ex.: dashboards em tempo real).
- **Regra:** autoriza somente se existir um registro em `Domain\Shared\Models\Platform\Platform` com a `public_key` informada.
- **Fallback:** se necessário, cacheie as plataformas com `cacheBasicData` para reduzir leituras no banco.

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

## Integrações de Cliente

### Vue 3 + Laravel Echo
1. Instale os pacotes necessários (`npm install laravel-echo pusher-js`) e inicialize o Echo dentro de `resources/js/bootstrap.js` (ou exponha um plugin dedicado) para reaproveitar o token do usuário logado.
2. Configure o plugin apontando para os envs do Vite e o endpoint `/broadcasting/auth` que valida os canais definidos aqui:

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

3. Utilize o Echo dentro dos componentes Vue 3 via Composition API. Lembre-se de sair do canal no `onBeforeUnmount`:

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

- Para `users.{uuid}` chame `echo.private(`users.${userUuid}`)`. Para `platform.{publicKey}` use o mesmo padrão, garantindo que a `public_key` exista.
- `Echo.private` adiciona o prefixo `private-` automaticamente, portanto o canal registrado aqui (`conversation.{uuid}`) equivale, no Pusher, a `private-conversation.{uuid}`.

### Serviços Rust (workers e integrações)
Serviços escritos em Rust podem consumir os mesmos canais privados via WebSockets do Pusher. A abordagem abaixo usa bibliotecas consolidadas (`tokio`, [`tokio-websockets`](https://docs.rs/tokio-websockets/latest/tokio_websockets/), `reqwest`) e evidencia como reutilizar a autorização de `/broadcasting/auth`.

1. Dependências mínimas:

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

2. Exemplo de assinatura do canal `conversation.{uuid}`:

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
    let backoffice_token = std::env::var("BACKOFFICE_TOKEN")?; // token com acesso aos canais privados
    let channel = format!("private-conversation.{conversation_uuid}");
    let ws_url = format!(
        "wss://ws-{cluster}.pusher.com/app/{app_key}?protocol=7&client=rust&version=0.1&flash=false"
    );
    let uri: Uri = ws_url.parse()?;

    let (mut socket, _) = ClientBuilder::from_uri(uri).connect().await?;
    let first = socket
        .next()
        .await
        .ok_or_else(|| anyhow!("sem handshake do Pusher"))??;
    let payload = first
        .as_text()
        .ok_or_else(|| anyhow!("payload inicial inválido"))?;
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
            println!("Evento recebido: {text}");
        }
    }

    Ok(())
}
```

- Ajuste `channel` para `private-platform.{publicKey}` quando estiver consumindo dados de plataforma.
- Para publicar eventos a partir de serviços Rust use o SDK `pusher-http-rust`, mas mantenha a autorização dos canais centralizada nas regras descritas acima.
- Armazene `backoffice_token` e outros segredos (APP KEY, cluster) em variáveis de ambiente ou no gerenciador de segredos do ambiente em vez de hardcode.

## Referências
- [Laravel Broadcasting](https://laravel.com/docs/10.x/broadcasting)
- `routes/channels.php`
- `Domain\Shared\Models\Chat\Conversation`
- `Domain\Shared\Models\Platform\Platform`
