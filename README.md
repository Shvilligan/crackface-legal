# Crackface Legal Site

Static GitHub-Pages / Cloudflare-Pages site that hosts the legal pages
for the **Crackface** mobile game.

## Files

- `index.html` — landing page with links
- `privacy-policy/index.html` — RU / EN privacy policy
- `terms-of-use/index.html` — RU / EN terms of use
- `support/index.html` — RU / EN support page (contact + FAQ)

> No `app-ads.txt` lives here. The developer-account-wide
> `app-ads.txt` is already hosted at
> https://shvilligan.github.io/app-ads.txt for the same AdMob
> publisher id — AdMob uses one Developer Site per account, not per
> app. Don't duplicate.

## Recommended hosting: Cloudflare Pages on `legal.crackface.app`

This site is meant to live at **https://legal.crackface.app** (sibling
to `assets.crackface.app`). Cloudflare Pages already handles DNS for
the rest of the `crackface.app` domain, so adding a subdomain is two
clicks.

### One-time setup

1. **Create a GitHub repository** (e.g. `crackface-legal`) and push
   this folder:

   ```bash
   git init
   git add .
   git commit -m "Initial legal pages"
   git branch -M main
   git remote add origin https://github.com/<YOUR_USERNAME>/crackface-legal.git
   git push -u origin main
   ```

2. **Create a Cloudflare Pages project**:
   - Cloudflare Dashboard → Workers & Pages → Create → Pages → Connect
     to Git → pick the `crackface-legal` repo
   - Build settings:
     - Production branch: `main`
     - Framework preset: **None**
     - Build command: *(leave empty)*
     - Build output directory: `/`
   - Save and deploy. First deploy lands at
     `crackface-legal.pages.dev` within ~30s.

3. **Bind the custom subdomain**:
   - In the Pages project → Custom domains → Set up a custom domain
   - Enter `legal.crackface.app`
   - Cloudflare auto-creates the CNAME if the `crackface.app` zone is
     on the same account (it is)
   - Activation takes ~1-2 minutes

4. **Verify**:
   - https://legal.crackface.app — landing
   - https://legal.crackface.app/privacy-policy/
   - https://legal.crackface.app/terms-of-use/
   - https://legal.crackface.app/support/

After this, every `git push` to `main` auto-redeploys the site.

## Updating content

- Edit the HTML directly. The pages are intentionally simple — no
  build step, no framework, no dependencies.
- When publishing a meaningful change to Privacy Policy or Terms of
  Use, bump the "Last updated" date at the top of the changed page.
  Bumping "Effective date" is reserved for changes that materially
  affect what data is collected or what users agree to — the in-app
  consent flow should re-prompt users when that happens.

## App-side wiring

The Crackface app links to these pages from its Settings screen
(English + Russian labels). The URLs are hard-coded; if you change
the hosting destination later, search `lib/screens/settings_screen.dart`
for `legal.crackface.app` and update accordingly.

The same URLs also need to land in the Play Console and App Store
Connect listings under "Privacy Policy URL" — both stores require it
to publish.

## `app-ads.txt`

AdMob's `app-ads.txt` for this publisher already lives at
**https://shvilligan.github.io/app-ads.txt** (covers all apps under
the same AdMob account). The Crackface app in the AdMob console
points its Developer Site at the same URL — nothing to change here.

If you ever switch publishers or split accounts, host a fresh
`app-ads.txt` somewhere and point each app's Developer Site at it
in **AdMob → Apps → \<App\> → Settings**.
