#### Description

The `oauth-app-x` command groups the subcommands of a deployable X (Twitter) OAuth service. The service holds your X application credentials (client id and, for a confidential Web App, secret) so that thin CLI clients never have to handle them.

It is a thin wrapper over `aux4/oauth`, pre-wired for X (x.com / api.x.com endpoints, space-separated scopes, HTTP Basic auth for confidential clients), and is designed to run as an `api`-type machine on aux4.cloud. Its routes are served behind an API Gateway:

- `GET /health` — liveness check.
- `GET /{provider}/authorize-url` — build the provider authorization URL server-side.
- `POST /{provider}/exchange` — exchange an authorization code for tokens server-side.
- `POST /{provider}/refresh` — renew an access token from a refresh token server-side.

The api runtime maps each request to the corresponding command, passing the request context as flags: path params on `--params`, the query string on `--query`, and the JSON body on `--body`. The commands read `${params.*}` / `${query.*}` / `${body.*}` accordingly.

Subcommands:

- **health** — liveness check.
- **authorize-url** — build the provider authorization URL server-side.
- **exchange** — exchange an authorization code for tokens server-side.
- **refresh** — renew an access token from a refresh token server-side.

#### Usage

```bash
aux4 oauth-app-x <subcommand>
```

#### Example

```bash
aux4 oauth-app-x authorize-url --params '{"provider":"x"}' --query '{"redirectUri":"http://127.0.0.1:9876/callback"}'
```

Served over HTTP as:

```bash
curl "https://<machine-url>/api/x/authorize-url?redirectUri=http://127.0.0.1:9876/callback"
```
