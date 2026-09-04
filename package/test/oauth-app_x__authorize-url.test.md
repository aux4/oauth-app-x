# oauth-app x authorize-url

This is the X provider plugin for the `aux4/oauth-app` core. Its command is
`aux4 oauth-app x authorize-url`, so the core must be installed. The api runtime
passes the query string on `--query`; the command reads `${query.scopes}` /
`${query.state}` and uses `X_CLIENT_ID`. The redirect is the broker's OWN callback
(`<base>/api/x/callback`), base from `BROKER_PUBLIC_URL` or the aux4.cloud-injected
`AUX4_CLOUD_VM_URL`.

## when X is configured

```execute
BROKER_PUBLIC_URL=https://broker.test/oauth-broker X_CLIENT_ID=test-cid aux4 oauth-app x authorize-url --query '{"scopes":"tweet.read users.read","state":"s"}'
```

```expect:partial
"url":"https://x.com/i/oauth2/authorize?response_type=code&client_id=test-cid
```

## when no scopes are requested (bundled default applied)

```execute
BROKER_PUBLIC_URL=https://broker.test/oauth-broker X_CLIENT_ID=test-cid aux4 oauth-app x authorize-url --query '{}'
```

```expect:partial
scope=tweet.read+users.read+offline.access
```

## the redirect targets the broker's own callback

```execute
BROKER_PUBLIC_URL=https://broker.test/oauth-broker X_CLIENT_ID=test-cid aux4 oauth-app x authorize-url --query '{}'
```

```expect:partial
redirect_uri=https%3A%2F%2Fbroker.test%2Foauth-broker%2Fapi%2Fx%2Fcallback
```

## the base falls back to the aux4.cloud-injected url when BROKER_PUBLIC_URL is unset

```execute
AUX4_CLOUD_VM_URL=https://aux4.on.aux4.cloud/oauth-broker X_CLIENT_ID=test-cid aux4 oauth-app x authorize-url --query '{}'
```

```expect:partial
redirect_uri=https%3A%2F%2Faux4.on.aux4.cloud%2Foauth-broker%2Fapi%2Fx%2Fcallback
```

## when the broker public url is not set

```execute
X_CLIENT_ID=test-cid aux4 oauth-app x authorize-url --query '{}'
```

```error:partial
public URL is not set
```

## when there are no credentials

```execute
BROKER_PUBLIC_URL=https://broker.test/oauth-broker X_CLIENT_ID= aux4 oauth-app x authorize-url --query '{}'
```

```error:partial
Error: x has no client credentials configured
```
