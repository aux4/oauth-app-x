#### Description

The `authorize-url` command builds a provider authorization URL on the server side, using the client id held by the broker (never exposed to the caller). It delegates to `aux4 oauth authorize-url`, supplying the provider's OAuth endpoints from the bundled `providers.yaml` and the client id from the machine environment.

It is served as `GET /{provider}/authorize-url`. The api runtime passes the request context as flags: the path params (`{provider}`) on `--params`, and the query string on `--query`. The command reads `${params.provider}`, `${query.redirectUri}`, `${query.scopes}`, and `${query.state}`.

The provider's client id is read from a per-provider environment variable by convention `<PROVIDER>_CLIENT_ID` (for example `X_CLIENT_ID`). If the provider is unknown or its credentials are not configured, the command exits with an error.

The command prints a JSON object with:

- `url` — the authorization URL to open in a browser.
- `codeVerifier` — the PKCE verifier the client must keep and pass to `exchange`.
- `state` — the opaque state value.

#### Usage

```bash
aux4 x-oauth-app authorize-url --params '{"provider":"<provider>"}' --query '{"redirectUri":"<uri>","scopes":"<scopes>","state":"<state>"}'
```

--params  Path params as JSON — must include `provider` (e.g. `x`)
--query   Query string as JSON — `redirectUri`, `scopes` (space/comma separated), `state`

The X client id is read from the `X_CLIENT_ID` environment variable. When the request omits `scopes`, the default comes from the `X_SCOPES` environment variable, and finally from the bundled X default (`tweet.read users.read offline.access`).

Served over HTTP as:

```bash
curl "https://<machine-url>/api/x/authorize-url?redirectUri=http://127.0.0.1:9876/callback&scopes=tweet.read%20users.read&state=xyz"
```

#### Example

```bash
aux4 x-oauth-app authorize-url --params '{"provider":"x"}' --query '{"redirectUri":"http://127.0.0.1:9876/callback","scopes":"tweet.read users.read","state":"xyz"}'
```

```json
{
  "url": "https://x.com/i/oauth2/authorize?response_type=code&client_id=...&code_challenge=...",
  "codeVerifier": "b7f3...",
  "state": "xyz"
}
```
