# aux4/oauth-app-x

Deploy your **own X (Twitter) OAuth service** in one click. It holds your X OAuth client id and secret server-side and does the authorization-URL build, the code exchange, and the token refresh on behalf of your CLI tools — so those tools **never handle a client secret**, and every user authenticates under **your** X app, quota, and consent screen.

It is a thin HTTP wrapper over [`aux4/oauth`](https://hub.aux4.io/r/public/packages/aux4/oauth), deployed as an `api`-type machine on [aux4.cloud](https://aux4.cloud). The package itself contains **no secrets** — your credentials are supplied as machine environment variables.

## Quick start

1. **Deploy it.** From the [hub package page](https://hub.aux4.io/r/public/packages/aux4/oauth-app-x), click **Deploy to cloud** (or `aux4 aux4 cloud deploy oauth-app-x --package aux4/oauth-app-x --api true`). You get a URL like `https://<your-scope>.on.aux4.cloud/oauth-app-x`.

2. **Create your X app.** In the [X developer portal](https://developer.x.com/), create an OAuth 2.0 app. Two client types work:
   - **Web App (confidential)** — has a **client secret**; the app authenticates to X's token endpoint with HTTP Basic auth. Set both env vars below.
   - **Native App (public)** — **no secret**, PKCE only. Set just `X_CLIENT_ID`.

   Either way the app's **Callback URI must be exactly `http://localhost:9876/callback`** and the type/website filled in.

3. **Add your credentials.**

   ```bash
   aux4 aux4 cloud oauth-app-x env set X_CLIENT_ID=... X_CLIENT_SECRET=...   # secret only for a Web App
   ```

   Values are encrypted at rest per scope and applied to the live machine immediately.

4. **Point your CLI at it.** A client asks the app for an authorization URL, sends the user through X's consent screen on a local loopback, and exchanges the code — for example:

   ```bash
   export X_AUTH_BROKER="https://<your-scope>.on.aux4.cloud/oauth-app-x/api"
   ```

   The client never sees the secret. Expired tokens are refreshed through the app automatically (X returns a refresh token when `offline.access` is requested).

## Installation

You do not normally install this package locally — you deploy it. To inspect or run it locally:

```bash
aux4 aux4 pkger install aux4/oauth-app-x
```

## How it works

The service exposes a liveness check plus three X endpoints, served under the `/api` prefix (for example `GET <machine-url>/api/health`):

1. `GET /health` — returns `{"status":"ok"}`.
2. `GET /x/authorize-url` — returns the X authorization URL, a PKCE `codeVerifier`, and the `state`.
3. `POST /x/exchange` — takes the authorization `code`, the `codeVerifier`, and the `redirectUri`, and returns the tokens plus the resolved profile.
4. `POST /x/refresh` — takes a `refreshToken` and returns a renewed set of tokens.

**X specifics** (baked in so you don't have to rediscover them):

- Endpoints use **`x.com` / `api.x.com`**, never `twitter.com` (which redirects and lowercases the query string, breaking `client_id` / `code_challenge`).
- Scopes are **space-separated**. The default is `tweet.read users.read offline.access` — `users.read` is required for the `/2/users/me` profile lookup done during the exchange, and `offline.access` for a refresh token.
- Confidential clients authenticate at the token endpoint with **HTTP Basic auth** (`clientSecretIn: basic`); public clients simply have no secret.

## Configuration

| Environment variable | Required | Description |
|----------------------|----------|-------------|
| `X_CLIENT_ID` | Yes | X OAuth client id |
| `X_CLIENT_SECRET` | For a Web App (confidential) client | X OAuth client secret. Omit for a Native App (public) client. |
| `X_SCOPES` | No | Default scopes (space or comma separated) used when a request does not specify any. Resolution is **request → `X_SCOPES` → the bundled default** (`tweet.read users.read offline.access`). |

Until `X_CLIENT_ID` is set, the endpoints return an error.

## Endpoints

### `GET /x/authorize-url`

Query parameters: `redirectUri` (loopback, e.g. `http://localhost:9876/callback`), `scopes` (space separated; defaults to `X_SCOPES` then the bundled default), `state`.

```bash
curl "https://<your-scope>.on.aux4.cloud/oauth-app-x/api/x/authorize-url?redirectUri=http://localhost:9876/callback"
```

```json
{
  "url": "https://x.com/i/oauth2/authorize?response_type=code&client_id=...&code_challenge=...",
  "codeVerifier": "b7f3...",
  "state": "..."
}
```

### `POST /x/exchange`

JSON body: `code`, `codeVerifier`, `redirectUri`.

```bash
curl -X POST "https://<your-scope>.on.aux4.cloud/oauth-app-x/api/x/exchange" \
  -H "Content-Type: application/json" \
  -d '{"code":"...","codeVerifier":"b7f3...","redirectUri":"http://localhost:9876/callback"}'
```

```json
{
  "accessToken": "...",
  "refreshToken": "...",
  "expiresIn": 7200,
  "tokenType": "bearer",
  "principal": { "data": { "id": "12", "name": "...", "username": "..." }, "provider": "x" }
}
```

### `POST /x/refresh`

JSON body: `refreshToken`.

```bash
curl -X POST "https://<your-scope>.on.aux4.cloud/oauth-app-x/api/x/refresh" \
  -H "Content-Type: application/json" \
  -d '{"refreshToken":"..."}'
```

## Security

These endpoints are **unauthenticated**: anything that can reach the URL can start an OAuth flow with your X app. That is by design for a loopback CLI login, but treat the deployment URL as semi-sensitive, register only the callback URI your clients use, and keep `X_SCOPES` scoped to what you need. A scope allowlist is a planned addition.
