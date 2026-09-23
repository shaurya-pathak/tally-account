# Tally account site

Public sign-in and OAuth 2.1 consent site for the Tally MCP server. It is
deployed on its own origin and intentionally holds no training data and no raw
motion traces.

- `index.html` — single-page sign-in and consent UI. Uses the Supabase
  publishable key, which is public by design; Row Level Security protects data.
- `oauth/consent/index.html` — stable `/oauth/consent` path that forwards the
  `authorization_id` to the single-page site.
- `404.html` — routes unknown paths back to the single-page site.

## Supabase settings that must match this origin

In the Tally Training project (`leqqijtojgxmnhoxuqec`):

- **Authentication → URL Configuration → Site URL**
  `https://shauryapathak.com/tally-account`
- **Authentication → URL Configuration → Redirect URLs**
  `https://shauryapathak.com/tally-account`, `tally://auth-callback`
- **Authentication → OAuth Server**
  enabled, Authorization Path `/oauth/consent`, dynamic client registration on.

These are declared in the main repo at `supabase/config.toml` and applied with
`supabase config push`.

## Deploy

GitHub Pages serves this directory from the repository root of `tally-account`
on the `main` branch. The canonical source lives in the main Tally repo at
`account-site/`; copy `index.html`, `404.html`, `README.md`, and `oauth/` to this
repository root when they change.
