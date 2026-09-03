# x-oauth-app authorize-url

The api runtime passes the request context as flags: path params on `--params`,
query string on `--query`. `authorize-url` reads `${params.provider}` and
`${query.redirectUri}` / `${query.scopes}` / `${query.state}`.

## when provider is missing

```execute
aux4 x-oauth-app authorize-url
```

```error:partial
Error: provider is required
```

## when provider is not configured

```execute
aux4 x-oauth-app authorize-url --params '{"provider":"github"}'
```

```error:partial
Error: provider 'github' is not configured
```

## when the configured provider has no credentials

```execute
X_CLIENT_ID= X_CLIENT_SECRET= aux4 x-oauth-app authorize-url --params '{"provider":"x"}' --query '{"redirectUri":"http://127.0.0.1:9876/callback"}'
```

```error:partial
Error: provider 'x' has no client credentials configured
```

## when X is configured

```execute
X_CLIENT_ID=test-client-id aux4 x-oauth-app authorize-url --params '{"provider":"x"}' --query '{"redirectUri":"http://127.0.0.1:9876/callback","scopes":"tweet.read users.read offline.access","state":"xyz"}'
```

```expect:partial
"url":"https://x.com/i/oauth2/authorize?response_type=code&client_id=test-client-id
```

## when X is configured with no requested scopes (defaults applied)

```execute
X_CLIENT_ID=test-client-id aux4 x-oauth-app authorize-url --params '{"provider":"x"}' --query '{"redirectUri":"http://127.0.0.1:9876/callback"}'
```

```expect:partial
scope=tweet.read+users.read+offline.access
```
