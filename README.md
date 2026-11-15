# The public site (for Google OAuth production)

Google will not let the OAuth app leave **Testing** without a public home page and privacy policy. These three
pages exist for that, and for nothing else. They are static, self-contained and need no build step.

```
site/
  index.html     home page
  privacy.html   privacy policy  → the URL Google asks for
  terms.html     terms of use
  styles.css     shared styles (light/dark, print)
  .nojekyll      keeps GitHub Pages from running Jekyll
```

The plugin's source repository is **private**. Do not publish it. Deploy only the contents of `site/`.

## Recommended: a separate public repo + GitHub Pages (free)

This keeps the source private and costs nothing.

1. Create an empty **public** repo named `thousand.turns` under your account (do not initialise it).
2. Copy the contents of `site/` to the root of that repo and push:

   ```sh
   cd /tmp && rm -rf thousand.turns && git clone https://github.com/aayushkedawat/thousand.turns.git
   cp -R /Users/aayushkedawat/Downloads/Projects/video_maker_plugin/site/. thousand.turns/
   cd thousand.turns && git add -A && git commit -m "site: home, privacy and terms" && git push
   ```

3. Repo → **Settings → Pages** → Source: *Deploy from a branch* → branch `main`, folder `/ (root)` → Save.
4. Wait for the deployment (a minute), then check:

   ```sh
   curl -sI https://aayushkedawat.github.io/thousand.turns/ | head -1
   curl -sI https://aayushkedawat.github.io/thousand.turns/privacy.html | head -1
   ```

   Both should answer `HTTP/2 200`.

URLs:

| What | URL |
|---|---|
| Home page | `https://aayushkedawat.github.io/thousand.turns/` |
| Privacy policy | `https://aayushkedawat.github.io/thousand.turns/privacy.html` |

## Alternative: Cloudflare Pages from the private repo (also free)

If GitHub rejects the `github.io` domain as an OAuth authorised domain:

1. Cloudflare dashboard → **Workers & Pages → Create → Pages → Connect to Git** → choose the private plugin repo.
2. Framework preset: **None**. Build command: empty. Build output directory: `site`.
3. Deploy. The URL is `https://<project>.pages.dev/` (privacy at `/privacy.html`).
4. If Google still refuses the domain, point a domain you own at the Pages project (a domain costs a few dollars a
   year; the hosting stays free).

## Google Cloud values

OAuth consent screen → **Branding**:

| Field | Value |
|---|---|
| App name | `Thousand Turns` |
| User support email | your own Google account email |
| App logo | optional; the pages do not depend on it |
| Authorised domain | `aayushkedawat.github.io` (or the `pages.dev` domain) |
| Developer contact email | your own Google account email |

Then **Audience → Publish app**, and on the next sign-in choose *Advanced → Go to Thousand Turns* once.

`privacy.html` is the **Privacy policy URL**; `index.html` (the folder URL) is the **Home page URL**.

## Before you click Publish

- Open both URLs in a private window; they must load with no login.
- The contact link points at `github.com/aayushkedawat/thousand.turns/issues` — the repo from step 1. Create it,
  or the link 404s.
- The pages say what the app actually does. If the app changes (new scope, new platform), update `privacy.html` and
  the effective date.

## What these pages must not claim

No account, no sign-up, no newsletter, no tracking, no server. That is true of the software, and it is why the
policy is short.
