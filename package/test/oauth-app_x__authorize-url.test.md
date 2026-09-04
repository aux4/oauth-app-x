# oauth-app x authorize-url

This is the X provider plugin for the `aux4/oauth-app` core. Its command is
`aux4 oauth-app x authorize-url`, so the core must be installed. The api runtime
passes the query string on `--query`; the command reads `${query.redirectUri}` /
`${query.scopes}` / `${query.state}` and uses `X_CLIENT_ID`.

## when X is configured

```execute
X_CLIENT_ID=test-cid aux4 oauth-app x authorize-url --query '{"redirectUri":"http://127.0.0.1:9876/callback","scopes":"tweet.read users.read","state":"s"}'
```

```expect:partial
"url":"https://x.com/i/oauth2/authorize?response_type=code&client_id=test-cid
```

## when no scopes are requested (bundled default applied)

```execute
X_CLIENT_ID=test-cid aux4 oauth-app x authorize-url --query '{"redirectUri":"http://127.0.0.1:9876/callback"}'
```

```expect:partial
scope=tweet.read+users.read+offline.access
```

## when there are no credentials

```execute
X_CLIENT_ID= aux4 oauth-app x authorize-url --query '{"redirectUri":"http://127.0.0.1:9876/callback"}'
```

```error:partial
Error: x has no client credentials configured
```
