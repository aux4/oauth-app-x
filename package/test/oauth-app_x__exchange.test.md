# oauth-app x exchange

`aux4 oauth-app x exchange` swaps an authorization code for tokens (the api runtime
passes the JSON body on `--body`; the command reads `${body.code}` /
`${body.codeVerifier}` / `${body.redirectUri}`).

## when there are no credentials

```execute
X_CLIENT_ID= X_CLIENT_SECRET= aux4 oauth-app x exchange --body '{"code":"abc","codeVerifier":"def","redirectUri":"http://127.0.0.1:9876/callback"}'
```

```error:partial
Error: x has no client credentials configured
```
