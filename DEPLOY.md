# Deployment runbook — ipda.world

Repo: <https://github.com/911511camel/IDPA>
Cloudflare Pages project: `ipda` → `https://ipda.pages.dev`
Domain: `ipda.world` (registered at GoDaddy)

## 1. GitHub

Remote is set to `git@github.com:911511camel/IDPA.git`.

Deploy key on this machine (add with **Allow write access**):

```
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIFJGK/nBWMC0mQmat/5pfQ3x1PkwFp1g4tAyeJ8QN0gw ipda-website-deploy
```

Repo → Settings → Deploy keys → Add deploy key → paste → tick *Allow write access*.

Then:

```bash
git push -u origin main
```

## 2. Cloudflare Pages (direct upload)

```bash
npx wrangler pages deploy dist --project-name=ipda --branch=main
```

`dist/` is the deploy artifact: `index.html`, `_headers`, `robots.txt`, `logo-*.png`.
Rebuild it after editing `index.html`:

```bash
cp index.html _headers robots.txt logo-mark.png logo-full.png dist/
```

## 3. Custom domain `ipda.world`

Cloudflare Pages needs DNS control for an apex (`@`) domain, and GoDaddy has no
CNAME flattening / ALIAS record. So the zone must live on Cloudflare:

1. Cloudflare dashboard → **Add a site** → `ipda.world` → Free plan.
2. Cloudflare shows two nameservers, e.g. `xxx.ns.cloudflare.com`.
3. GoDaddy → **My Products → ipda.world → DNS → Nameservers → Change → I'll use my own**
   → enter both Cloudflare nameservers → save.
4. Wait for propagation (minutes to a few hours; Cloudflare emails when the zone is active).
5. Cloudflare → **Workers & Pages → ipda → Custom domains → Set up a domain**
   → `ipda.world` and `www.ipda.world` → both are created as CNAMEs automatically.

Do **not** point GoDaddy's DNS at `ipda.pages.dev` directly: the apex would need an
A record, which Pages cannot provide, and SSL would fail.

### If the zone must stay on GoDaddy

Keep `ipda.world` parked and serve only `www.ipda.world`:

- GoDaddy DNS: CNAME `www` → `ipda.pages.dev`
- GoDaddy **Forwarding**: `ipda.world` → `https://www.ipda.world`
- Cloudflare Pages → Custom domains → add only `www.ipda.world`

## 4. After the first deploy

- Test `https://ipda.world` for console errors (Tailwind CDN + CSP is the risky part;
  `_headers` allows `cdn.tailwindcss.com`, `fonts.googleapis.com`, `data:` images, inline scripts).
- Add `meta description`, Open Graph tags and a favicon before promoting the domain — link
  previews are currently blank.
