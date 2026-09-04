# aux4/oauth-app-x 0.0.6

## Changed

- **Endpoint auth alignment with the secure-by-default core.** `/x/callback` and
  `/x/refresh` are marked `public: true` (the browser redirect must reach the
  callback without a token, and a refresh token is itself the credential), so they
  stay open. `/x/authorize-url` and `/x/exchange` inherit the core's
  secure-by-default gate — a caller must present a valid aux4 idToken unless the VM
  runs with `OAUTH_APP_PUBLIC=true`.
