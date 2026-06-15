# Offensive Questions — public-facing legal pages

Three HTML files ready to paste anywhere static. Apple App Review needs
public URLs (no login wall) for:

- Privacy Policy → required in ASC App Information
- Terms of Use → required if you offer auto-renewable subscriptions
- Support URL → required field on the version submission page

The in-app screens still fetch from `api.offensivequestions.com/legals/*`
unchanged — these standalone files are only for the App Store Connect
URL fields and the world-facing browser-visible versions.

## Fastest path to public URLs — GitHub Pages

If you have a GitHub account (Ivy-Insights org would work):

```bash
# from the legal/ directory
cd ~/oq-review/legal
git init
git add .
git commit -m "Initial: legal + support pages for App Store review"
gh repo create Ivy-Insights/offensive-questions-legal --public --source=. --push
gh api /repos/Ivy-Insights/offensive-questions-legal/pages \
  -X POST \
  -f source[branch]=main \
  -f source[path]=/ \
  >/dev/null
```

Pages goes live within ~1 minute at:

- https://ivy-insights.github.io/offensive-questions-legal/
- https://ivy-insights.github.io/offensive-questions-legal/privacy-policy.html
- https://ivy-insights.github.io/offensive-questions-legal/terms-of-use.html
- https://ivy-insights.github.io/offensive-questions-legal/support.html

Use those URLs in ASC.

## Alternative: Netlify drop (no GitHub needed)

1. Open https://app.netlify.com/drop
2. Drag the `~/oq-review/legal` folder into the browser window
3. Netlify gives you a `*.netlify.app` URL — use that

## Alternative: Cloudflare Pages, S3, Vercel, etc.

Any static-hosting platform will work. Just upload the four files and
make sure they're served as `text/html`.

## After hosting

Update App Store Connect with the URLs:

- ASC → My Apps → Offensive Questions → **App Information** →
  **Privacy Policy URL** → paste the privacy-policy.html URL
- ASC → My Apps → Offensive Questions → **App Information** →
  scroll for the **Marketing URL** (optional, can paste the index)
- ASC → My Apps → Offensive Questions → 1.0.1 version page →
  **Support URL** field → paste the support.html URL

## Files

| File | Purpose |
|------|---------|
| `index.html` | Landing page linking to all three |
| `privacy-policy.html` | Synced from `api.offensivequestions.com/legals/privacy-policy` at 2026-06-15 |
| `terms-of-use.html` | Synced from `api.offensivequestions.com/legals/terms-of-use` at 2026-06-15 |
| `support.html` | Custom support FAQ + contact email |

## Keep in sync

If the backend `/legals/*` content changes, regenerate by running the
extraction snippet from the deploy commit message in
`offensive-questions-mobile` (or just re-curl the endpoints, extract
`.text` from the JSON, drop into the HTML body).

## Caveats

The support page advertises `support@offensivequestions.com` as the
contact email. If that mailbox isn't actually monitored, Apple may
notice and reject — make sure something receives email at that address
before submitting.
