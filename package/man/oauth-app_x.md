#### Description

The `x` command is the X (Twitter) provider of the `aux4/oauth-app` core. This package is a **plugin**: it depends on `aux4/oauth-app`, adds the `x` command under the shared `oauth-app` profile, and contributes the `/x/*` routes. Deploy it (with the core) to serve X OAuth; deploy it alongside other provider plugins to serve several providers from one machine.

Subcommands (served under `/x/…`):

- **authorize-url** — build the X authorization URL server-side.
- **exchange** — exchange an authorization code for tokens server-side.
- **refresh** — renew an access token from a refresh token server-side.

Credentials are machine environment variables: `X_CLIENT_ID` and, for a confidential (Web App) client, `X_CLIENT_SECRET` (sent to X's token endpoint via HTTP Basic auth). A public (Native App) client needs only `X_CLIENT_ID`. `X_SCOPES` sets default scopes.

#### Usage

```bash
aux4 oauth-app x <subcommand>
```

#### Example

```bash
aux4 oauth-app x authorize-url --query '{"redirectUri":"http://127.0.0.1:9876/callback"}'
```
