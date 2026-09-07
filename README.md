# refinix-site

The public Refinix site, served at **https://refinix.runs-on.dev**.

This repo is a build output, not a source of truth. Every file here is
generated from `frontend/design/` in the AegisForge repo by:

```bash
scripts/build-site.sh
```

Edit the pages there, re-run that script, then commit and push here. Editing
files in this repo directly means the next build silently overwrites them.

## What's in the bundle

| Path | From | Role |
|---|---|---|
| `index.html` | `frontend/design/site.html` | Landing page, including the `#get` download section |
| `docs.html` | `frontend/design/docs.html` | Documentation page (its `site.html` links are retargeted to `index.html`) |
| `site.css`, `docs.css` | same names | Styles |
| `assets/` | `frontend/design/assets/` | Only the files the two pages actually reference |
| `CNAME` | generated | `refinix.runs-on.dev` — GitHub's custom-domain mechanism |
| `.nojekyll` | generated | Serve the HTML as-is, no Jekyll processing |

The app-surface mockups (chat, code, control, documents, onboarding, pairing)
are deliberately not published — they are unreleased product surfaces and
nothing on the landing page links to them.

`assets/refinix-lockup-h.png` and `assets/refinix-lockup-v.png` are referenced
but intentionally absent. Both `<img>` tags carry `onerror="this.remove()"`, so
the in-page SVG lockup shows instead. Dropping real files in at those paths
replaces the drawn lockup without any code change.

## How the domain is wired

Two independent halves have to agree, and it does not matter which lands first
— GitHub just won't issue a certificate until both point at each other.

1. **The registry.** `domains/refinix.json` in the public runs-on.dev registry
   carries `"records": { "CNAME": "vedantsur09.github.io" }`. Set it at
   https://runs-on.dev/manage or by pull request.
2. **This repo.** The `CNAME` file above, plus Pages enabled on the default
   branch at root.

Confirm it worked in Settings → Pages ("DNS check successful"), then enable
Enforce HTTPS once the certificate is issued.
