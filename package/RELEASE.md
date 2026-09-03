# aux4/x-oauth-app 0.0.1

Initial release: a public, one-click-deployable **X (Twitter) OAuth service**.

- Deploy it to your own aux4.cloud scope, set `X_CLIENT_ID` (and `X_CLIENT_SECRET`
  for a Web App / confidential client), and point your CLI tools at its URL.
- Routes: `GET /health`, `GET /x/authorize-url`, `POST /x/exchange`,
  `POST /x/refresh` — the client secret stays server-side at login and refresh.
- Pre-wired for X's quirks: `x.com` / `api.x.com` endpoints, space-separated
  scopes (default `tweet.read users.read offline.access`), and `clientSecretIn:
  basic` (HTTP Basic auth at the token endpoint, required by X confidential
  clients; ignored for public/PKCE clients).
- `X_SCOPES` sets default scopes when a request does not specify any.

Requires `aux4/oauth` ≥ 0.1.4 (the `clientSecretIn` support). Derived from
`aux4/google-oauth-app`.
