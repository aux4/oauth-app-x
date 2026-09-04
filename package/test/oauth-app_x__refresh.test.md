# oauth-app x refresh

`aux4 oauth-app x refresh` renews an access token from a refresh token (the api
runtime passes the JSON body on `--body`; the command reads `${body.refreshToken}`).

## when the refresh token is missing

```execute
X_CLIENT_ID=test-cid X_CLIENT_SECRET=sec aux4 oauth-app x refresh --body '{}'
```

```error:partial
Error: refreshToken is required
```

## when there are no credentials

```execute
X_CLIENT_ID= X_CLIENT_SECRET= aux4 oauth-app x refresh --body '{"refreshToken":"rt"}'
```

```error:partial
Error: x has no client credentials configured
```
