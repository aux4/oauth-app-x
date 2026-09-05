# aux4/oauth-app-x 0.0.7

## Fixed

- **X code exchange failed on a multi-package broker** with `no tokenUrl for
  provider 'x'`. The authorize-url, exchange and refresh commands now pass X's
  endpoints (`--authUrl` / `--tokenUrl` / `--userinfoUrl`) and `--clientSecretIn
  basic` explicitly, in addition to `--configFile providers.yaml`. Explicit flags
  take precedence, so the token operations no longer depend on the `providers.yaml`
  resolving on the deployed VM.
