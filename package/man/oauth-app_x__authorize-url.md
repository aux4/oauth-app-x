#### Description

Builds the X authorization URL on the server side using the client id held by this app (never exposed to the caller). It delegates to `aux4 oauth authorize-url` with X's endpoints from the bundled `providers.yaml` and the client id from `X_CLIENT_ID`.

Served as `GET /x/authorize-url`. The api runtime passes the query string on `--query`; the command reads `${query.redirectUri}`, `${query.scopes}`, `${query.state}`. When `scopes` is omitted, the default comes from `X_SCOPES`, then the bundled default (`tweet.read users.read offline.access`). If `X_CLIENT_ID` is not set, it errors.

Prints `{ url, codeVerifier, state }`.

#### Usage

```bash
aux4 oauth-app x authorize-url --query '{"redirectUri":"<uri>","scopes":"<scopes>","state":"<state>"}'
```

#### Example

```bash
aux4 oauth-app x authorize-url --query '{"redirectUri":"http://127.0.0.1:9876/callback","state":"xyz"}'
```

```json
{
  "url": "https://x.com/i/oauth2/authorize?response_type=code&client_id=...&code_challenge=...",
  "codeVerifier": "b7f3...",
  "state": "xyz"
}
```
