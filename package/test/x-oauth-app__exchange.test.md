# x-oauth-app exchange

The api runtime passes the request context as flags: path params on `--params`,
the JSON request body on `--body`. `exchange` reads `${params.provider}` and
`${body.code}` / `${body.codeVerifier}` / `${body.redirectUri}`.

## when provider is missing

```execute
aux4 x-oauth-app exchange --body '{"code":"abc","codeVerifier":"def","redirectUri":"http://127.0.0.1:9876/callback"}'
```

```error:partial
Error: provider is required
```

## when provider is not configured

```execute
aux4 x-oauth-app exchange --params '{"provider":"github"}' --body '{"code":"abc","codeVerifier":"def","redirectUri":"http://127.0.0.1:9876/callback"}'
```

```error:partial
Error: provider 'github' is not configured
```

## when the configured provider has no credentials

```execute
X_CLIENT_ID= X_CLIENT_SECRET= aux4 x-oauth-app exchange --params '{"provider":"x"}' --body '{"code":"abc","codeVerifier":"def","redirectUri":"http://127.0.0.1:9876/callback"}'
```

```error:partial
Error: provider 'x' has no client credentials configured
```
