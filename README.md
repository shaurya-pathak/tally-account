# Tally account site

Public sign-in, OAuth 2.1 consent, and invite landing site for Tally. It holds
no workout records or raw motion traces.

- `index.html` — single-page sign-in and consent UI. Uses the Supabase
  publishable key, which is public by design; Row Level Security protects data.
- `assets/tally-mark.svg` — shared Tally mark and site favicon.
- `oauth/consent/index.html` — stable `/oauth/consent` path that forwards the
  `authorization_id` to the single-page site.
- `404.html` — routes unknown paths back to the single-page site.
- Invite URLs use `/?invite=<code>`; **Open Tally** launches
  `tally://invite/<code>`. Set the TestFlight URL in `index.html` when remote
  distribution is ready.
- Privacy copy covers social profiles, audience-scoped workout posts, private
  workout photos, and on-device sensor/Health data.

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

The canonical source lives here in `account-site/`. On a push to `main` that
changes this directory, the `Deploy account site` workflow mirrors it into the
root of the public `tally-account` repository. GitHub Pages serves that
repository from its `main` branch. The first deployment needs a fine-grained
token named `TALLY_ACCOUNT_DEPLOY_TOKEN` with **Contents: read and write** on
`shaurya-pathak/tally-account`; add it as a repository Actions secret on the
private `tally` repository. See the [CI/CD setup guide](https://github.com/shaurya-pathak/tally/blob/main/docs/CI_CD.md).
