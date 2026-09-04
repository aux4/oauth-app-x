#### Description

Exchanges an authorization code for X tokens on the server side, using the client id and secret held by this app. It delegates to `aux4 oauth exchange --includeTokens true` with X's endpoints from the bundled `providers.yaml`. Confidential (Web App) clients authenticate to X's token endpoint with HTTP Basic auth (`clientSecretIn: basic`); public clients use PKCE only.

Served as `POST /x/exchange`. The api runtime passes the JSON body on `--body`; the command reads `${body.code}`, `${body.codeVerifier}`, `${body.redirectUri}`. If `X_CLIENT_ID` is not set, it errors.

Prints the tokens plus the resolved profile.

#### Usage

```bash
aux4 oauth-app x exchange --body '{"code":"<code>","codeVerifier":"<verifier>","redirectUri":"<uri>"}'
```

#### Example

```bash
aux4 oauth-app x exchange --body '{"code":"...","codeVerifier":"...","redirectUri":"http://127.0.0.1:9876/callback"}'
```

```json
{
  "accessToken": "...",
  "refreshToken": "...",
  "expiresIn": 7200,
  "tokenType": "bearer",
  "principal": { "data": { "id": "12", "username": "..." }, "provider": "x" }
}
```
