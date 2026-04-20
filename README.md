# contractfocus-site

Static marketing site for the ContractFocus Chrome extension, served at https://contractfocus.app.

Four pages, plain HTML, no build step, no framework:

- `index.html` — landing page (hero, features, 3 differentiators, 5 clause packs, pricing, CTAs for setup and team)
- `privacy.html` — privacy policy (URL submitted to the Chrome Web Store)
- `setup.html` — Chrome AI / Gemini Nano setup guide for Prompt and Summarizer APIs
- `team.html` — owner-pays Team plan setup and seat management

## Deploy to Vercel via GitHub

1. Create a new public repo on GitHub under the Loophead Labs account, named `contractfocus-site`.
2. From this folder:
   ```
   git init
   git add .
   git commit -m "Initial site: homepage, privacy, Chrome AI setup, Team setup"
   git branch -M main
   git remote add origin git@github.com:<your-account>/contractfocus-site.git
   git push -u origin main
   ```
3. In the Vercel dashboard, click **Add New → Project**, import the `contractfocus-site` repo, accept the defaults (Vercel reads `vercel.json`), and deploy.
4. In the Vercel project settings, open **Domains** and add `contractfocus.app` (and optionally `www.contractfocus.app`).
5. At your DNS provider for `contractfocus.app`:
   - Apex record: either an `A` record to `76.76.21.21`, or an `ALIAS/ANAME` to `cname.vercel-dns.com`.
   - `www` CNAME to `cname.vercel-dns.com`.
6. Vercel provisions a TLS certificate automatically. Give it a minute, then visit https://contractfocus.app.

Once DNS propagates:

- https://contractfocus.app serves the homepage
- https://contractfocus.app/privacy serves the privacy policy (this is the URL that goes into the Chrome Web Store "Privacy policy URL" field)
- https://contractfocus.app/setup serves the Chrome AI setup guide (linked from the side panel when on-device AI is unavailable)
- https://contractfocus.app/team serves the Team plan setup and seat management guide (linked from the Team tab in the side panel)

The `cleanUrls` setting in `vercel.json` means the `.html` suffix is optional; both `/privacy` and `/privacy.html` resolve to the same page.

## Local preview

```
cd contractfocus-site
python3 -m http.server 4000
```

Open http://localhost:4000 in your browser.

## Editing copy

All four pages are single self-contained HTML files with inline `<style>` blocks. Edit the HTML directly. CSS variables at the top of each file (`--brand`, `--accent`, `--bg`, `--ink`, etc.) control the brand palette. The ContractFocus brand color is `#0e7c66` (deep teal) and the accent color is `#ba7a1e` (amber highlighter), matching the extension's side panel.

The brand-mark (a miniature document with a highlighter streak) is rendered via CSS pseudo-elements on `.brand-mark` so no image file is needed in the page header. The same design is also emitted as an inline SVG favicon via the `<link rel="icon">` data URL.

## Pricing reference

Any time the site references prices, these are the canonical values, mirrored from the extension's build spec and the live ExtensionPay plan configuration:

| Tier        | Price              | Notes                                             |
| ----------- | ------------------ | ------------------------------------------------- |
| Free        | $0                 | 3 analyses per day, 1 clause pack (auto-renewal)  |
| Pro monthly | $6.99 per month    | Unlimited analyses, all 5 clause packs            |
| Pro annual  | $49 per year       | Same as Pro monthly, billed annually              |
| Team annual | $99 per year       | Owner pays once, up to 5 teammates at no extra cost |

Never invent or adjust these in copy; if the spec changes, update both the build spec and this site in the same pass.

## Files

| File            | Purpose                                                 |
| --------------- | ------------------------------------------------------- |
| `index.html`    | Homepage                                                |
| `privacy.html`  | Privacy policy                                          |
| `setup.html`    | Chrome AI / Gemini Nano setup guide                     |
| `team.html`     | Team plan setup and seat management guide               |
| `vercel.json`   | Vercel build config with clean URLs and security headers |
| `CNAME`         | Legacy GitHub Pages domain binding (harmless on Vercel) |
| `.nojekyll`     | Legacy GitHub Pages marker (harmless on Vercel)         |
| `.gitignore`    | Ignore OS, editor, and Vercel build artifacts           |
