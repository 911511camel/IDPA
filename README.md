# IPDA Philippines — Website

Static single-page site for the **International Partnership and Development Association of the Philippines, Inc. (IPDA Philippines)**.

**Live:** <https://ipda.world>

- `index.html` — the entire site (styles, scripts and photographs are embedded, so it stays self-contained)
- `logo-mark.png` — emblem only, transparent background
- `logo-full.png` — full lockup, transparent background
- `_headers` — security and caching headers
- `wrangler.jsonc` — Cloudflare Worker config (assets + custom domains)
- `DEPLOY.md` — deployment runbook

## Local preview

```bash
python3 -m http.server 5500
# http://127.0.0.1:5500/
```

## Deploy

```bash
cp index.html _headers robots.txt logo-mark.png logo-full.png dist/
CLOUDFLARE_API_TOKEN=$(cat ~/.config/ipda/cloudflare-911511) \
CLOUDFLARE_ACCOUNT_ID=2520726af8a73ddd60097700ebec6562 \
npx wrangler deploy
```

See `DEPLOY.md` for details, DNS and certificate notes.
