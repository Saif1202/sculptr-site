# sculptr-app.com

The public landing page, plus the legal pages the app links to. Everything is
static: no build step, no dependencies.

```
index.html            landing page
privacy-policy.html   linked from the app and both store listings
terms.html
support.html          Support URL in App Store Connect
delete-account.html   account deletion URL required by Google Play
confirmed.html        email-confirmation landing
assets/               logo, app icon, screenshots (web-sized, ~320 KB total)
CNAME                 custom domain for GitHub Pages
.nojekyll             stops GitHub Pages ignoring files
```

The legal pages must stay on this domain: the app hardcodes
`https://sculptr-app.com/privacy-policy.html` and `/support.html`
(see `src/lib/config.ts`), and both stores point at the same URLs.

## Add the store links

Open `index.html` and edit the two lines near the bottom:

```js
var APP_STORE_URL   = "";   // "https://apps.apple.com/gb/app/sculptr/id0000000000"
var GOOGLE_PLAY_URL = "";   // "https://play.google.com/store/apps/details?id=com.sculptr.app"
```

A filled URL renders a live badge. An empty one renders a non-clickable
"Coming soon", so Android can stay pending until it goes live.

## Deploy to GitHub Pages

1. Create a public repo, e.g. `sculptr-site`.
2. Copy the contents of this folder (not the folder itself) into it and push:
   ```bash
   git init && git add -A && git commit -m "Launch sculptr-app.com" && git push
   ```
3. Repo → Settings → Pages → Source: `Deploy from a branch`, branch `main`, folder `/ (root)`.
4. Settings → Pages → Custom domain: `sculptr-app.com`, then tick **Enforce HTTPS**
   once the certificate is issued (can take a few minutes).

## DNS

At your domain registrar, point the apex at GitHub Pages:

| Type  | Name | Value                                        |
|-------|------|----------------------------------------------|
| A     | @    | 185.199.108.153                              |
| A     | @    | 185.199.109.153                              |
| A     | @    | 185.199.110.153                              |
| A     | @    | 185.199.111.153                              |
| CNAME | www  | `<your-github-username>.github.io`           |

DNS can take up to an hour. Until HTTPS is enforced the app's in-app links will
still work over http, but enable it as soon as the certificate appears.

## Updating

Edit the HTML and push. To regenerate the screenshots after a redesign:

```bash
node scripts/make-screenshots.js     # in the app repo
```

then re-run the resize step that produced `assets/` (520px wide PNGs).
