# aux4/oauth-app-x

The **X (Twitter) provider plugin** for the [`aux4/oauth-app`](https://hub.aux4.io/r/public/packages/aux4/oauth-app) core. It depends on the core, adds the `x` command under the shared `oauth-app` profile (`aux4 oauth-app x …`), and contributes the `/x/*` routes. Deploy it on top of the core to give your CLI tools X OAuth without ever handling a client secret.

## Installation

```bash
aux4 aux4 pkger install aux4/oauth-app-x
```

## Deploying

```bash
# X only
aux4 aux4 cloud deploy oauth --package aux4/oauth-app-x --api true \
  --env X_CLIENT_ID=... --env X_CLIENT_SECRET=...

# alongside other providers on one machine
aux4 aux4 cloud deploy oauth \
  --package aux4/oauth-app-google --package aux4/oauth-app-x --api true \
  --env X_CLIENT_ID=... --env X_CLIENT_SECRET=...
```

Installing the plugin pulls the `aux4/oauth-app` core (which provides `/health` and the shared api/oauth machinery).

## Configuration

| Environment variable | Required | Description |
|----------------------|----------|-------------|
| `X_CLIENT_ID` | Yes | X OAuth client id |
| `X_CLIENT_SECRET` | For a confidential (Web App) client | Sent to X's token endpoint via HTTP Basic auth (`clientSecretIn: basic`). Omit for a public (Native App) client. |
| `X_SCOPES` | No | Default scopes when a request omits them (request → `X_SCOPES` → bundled default `tweet.read users.read offline.access`). |

## Routes

Served under the `/api` prefix on the deployed machine:

- `GET /x/authorize-url` — build the X authorization URL. **Gated** by the endpoint auth.
- `GET /x/callback` — the browser redirect landing point (delegates to the core `callback` handler). **Public** — the provider redirect carries no aux4 token. Register `<machine-url>/api/x/callback` as the redirect URI on the X app.
- `POST /x/exchange` — exchange an authorization code for tokens. **Gated**.
- `POST /x/refresh` — renew an access token from a refresh token. **Public but rate-limited** (the refresh token is itself the credential).

## Security

The broker is **secure by default**: `authorize-url` and `exchange` require a valid aux4 idToken whose owner is entitled to the machine's scope, enforced by the core's endpoint-auth gate (see [`aux4/oauth-app` › Endpoint authentication](https://hub.aux4.io/r/public/packages/aux4/oauth-app)). `callback` is public and `refresh` is public but rate-limited. Set `OAUTH_APP_PUBLIC=true` on the machine to run it fully public.

## X specifics

Uses `x.com` / `api.x.com` (never `twitter.com`, which lowercases the query string and breaks `client_id`/`code_challenge`), space-separated scopes, and the loopback callback `http://localhost:9876/callback` — so the X app's callback URI must match exactly, and confidential apps use HTTP Basic auth at the token endpoint.
