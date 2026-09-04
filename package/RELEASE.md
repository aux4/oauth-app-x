# aux4/oauth-app-x 0.0.5

## Changed

- **Hosted-callback + poll flow.** `authorize-url`/`exchange` now redirect to the broker's own HTTPS callback (`<base>/api/x/callback`) instead of a localhost loopback — works from **any device**, and satisfies X's confidential-client rule (X rejects `localhost` redirects for confidential/Web App clients). Adds the `GET /x/callback` route (owned by this plugin, handled by the core).
- **Zero-config URL on aux4.cloud.** The redirect base resolves as `nvl(BROKER_PUBLIC_URL, AUX4_CLOUD_VM_URL)`: an aux4.cloud machine self-configures; `BROKER_PUBLIC_URL` remains an override for self-hosters (base only, no `/api`).

## Notes

- Requires `aux4/oauth-app` ≥ 0.0.4. The X app must be **Web App (confidential)** with **Read and write** permissions and `<base>/api/x/callback` registered as a Callback URI.
