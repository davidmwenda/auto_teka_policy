# Publishing the OTOTEKA privacy policy (from `develop`)

This folder is a static site. It does not change the Flutter app, Supabase, or the Vercel admin deploy.

Publish from **`develop`**, not `main`. Do not merge this into `main` only to host the policy.

## Why this setup

- `docs/` holds internal notes. Do **not** set Pages to “Deploy from a branch” + `/docs`.
- Admin production deploy (`.github/workflows/deploy-admin.yml`) still targets Vercel on `main` for `auto-teka-admin/**` only. This privacy workflow is separate.
- GitHub’s branch-folder Pages option only allows `/` or `/docs`. The policy lives in `auto-teka-app/privacy-policy`, so use **GitHub Actions** and `.github/workflows/privacy-policy-pages.yml`.
- That workflow uploads **only** `auto-teka-app/privacy-policy` and triggers on pushes to **`develop`**.

## Permissions the workflow needs

In the YAML (already set):

| Permission | Why |
|---|---|
| `contents: read` | Checkout the repo |
| `pages: write` | Publish the Pages artifact |
| `id-token: write` | OIDC for `actions/deploy-pages` |

In the repository UI you must also set **Settings → Pages → Source → GitHub Actions**. The first deploy may ask you to approve the **`github-pages`** environment.

## Before you publish

1. Confirm the yellow “À confirmer” items on the policy page (legal entity, monitored email).
2. Get these paths onto **`develop`** (PR or merge from `feature/frontend-david` into `develop` — not into `main`):
   - `auto-teka-app/privacy-policy/**`
   - `.github/workflows/privacy-policy-pages.yml`

They are currently on `feature/frontend-david` and **not** on `origin/develop` until you merge/push.

## Steps (GitHub UI)

1. Open [https://github.com/davidmwenda/auto_teka](https://github.com/davidmwenda/auto_teka).
2. Merge the privacy-policy commit(s) into **`develop`** (do not use `main` for hosting).
3. **Settings → Pages**.
4. **Build and deployment → Source** = **GitHub Actions**. Do not choose “Deploy from a branch”.
5. Approve the **`github-pages`** environment if GitHub asks.
6. **Actions → Publish privacy policy**. It should start on the push to `develop`. If not, open the workflow and use **Run workflow**, selecting branch **`develop`**.
7. When the job is green, open:

   - Policy: `https://davidmwenda.github.io/auto_teka/`
   - Deletion: `https://davidmwenda.github.io/auto_teka/suppression.html`

## Verify after publication

1. Private / logged-out browser window.
2. Policy page shows “Politique de confidentialité” over **HTTPS**.
3. Footer link opens `suppression.html`.
4. No GitHub login wall (repo must allow a public Pages site).

Paste the policy URL into Google Play Console → Privacy policy. Use the suppression URL for account deletion.

## Note on `workflow_dispatch`

Manual “Run workflow” in the Actions UI is easiest when the workflow file also exists on the **default branch**. Push/merge to **`develop`** is enough for the automatic `push` trigger. Prefer that path for this repo so you never need `main` for Pages.
