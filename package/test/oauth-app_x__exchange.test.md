# oauth-app x exchange

`aux4 oauth-app x exchange` swaps an authorization code for tokens (the api runtime
passes the JSON body on `--body`; the command reads `${body.code}` /
`${body.codeVerifier}`). The redirect URI is fixed to the broker's own callback
(`<base>/api/x/callback`) so it matches the one used at authorize.

## when the broker public url is not set

```execute
X_CLIENT_ID=cid X_CLIENT_SECRET=sec aux4 oauth-app x exchange --body '{"code":"abc","codeVerifier":"def"}'
```

```error:partial
public URL is not set
```

## when there are no credentials

```execute
BROKER_PUBLIC_URL=https://broker.test/oauth-broker X_CLIENT_ID= X_CLIENT_SECRET= aux4 oauth-app x exchange --body '{"code":"abc","codeVerifier":"def"}'
```

```error:partial
Error: x has no client credentials configured
```
