#### Description

Renews an X access token from a refresh token on the server side, using the client id and secret held by this app. It delegates to `aux4 oauth refresh` with X's endpoints from the bundled `providers.yaml` (confidential clients use HTTP Basic auth).

Served as `POST /x/refresh`. The api runtime passes the JSON body on `--body`; the command reads `${body.refreshToken}`. Errors if the refresh token is missing or `X_CLIENT_ID` is not set.

#### Usage

```bash
aux4 oauth-app x refresh --body '{"refreshToken":"<token>"}'
```

#### Example

```bash
aux4 oauth-app x refresh --body '{"refreshToken":"..."}'
```

```json
{
  "accessToken": "...",
  "refreshToken": "...",
  "expiresIn": 7200,
  "tokenType": "bearer"
}
```
