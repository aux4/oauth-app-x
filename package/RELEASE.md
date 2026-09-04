# aux4/oauth-app-x

Restructured into a PLUGIN of the aux4/oauth-app core (was a standalone app). It
now depends on aux4/oauth-app, adds the `x` command under the shared `oauth-app`
profile (aux4 oauth-app x ...), and contributes the /x/* routes. The core provides
/health and the shared api/oauth machinery. Set X_CLIENT_ID / X_CLIENT_SECRET
(+ optional X_SCOPES); confidential clients use HTTP Basic auth (clientSecretIn: basic).
