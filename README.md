# IPDA Philippines — Website

Static single-page site for the **International Partnership and Development Association of the Philippines, Inc. (IPDA Philippines)**.

- `index.html` — the entire site (styles, scripts and photographs are embedded, so it stays self-contained)
- `logo-mark.png` — emblem only, transparent background
- `logo-full.png` — full lockup, transparent background
- `_headers` — security and caching headers for Cloudflare Pages
- `robots.txt`

## Local preview

```bash
python3 -m http.server 5500
# http://127.0.0.1:5500/
```

## GitHub Pages

Settings → Pages → Source: *Deploy from a branch* → `main` / `/ (root)`.

## Cloudflare Pages

```bash
npx wrangler pages deploy . --project-name=ipda-philippines
```

The `_headers` file is applied automatically by Cloudflare Pages.
