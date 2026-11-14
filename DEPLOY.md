# Deployment runbook — ipda.world

| Part | Where |
|---|---|
| Repo | <https://github.com/911511camel/IDPA> (`main`) |
| Hosting | Cloudflare Workers `ipda`, account **911511@gmail.com** (`2520726af8a73ddd60097700ebec6562`) |
| Live | <https://ipda.world> · <https://www.ipda.world> |
| Staging | <https://ipda.911511.workers.dev> |
| DNS | zone `ipda.world` in the same account, NS `bethany`/`kevin.ns.cloudflare.com` |
| Cert | Google Trust Services, auto-renewed |

## Deploy

`dist/` is the upload artifact; rebuild it before every deploy:

```bash
cp index.html _headers robots.txt logo-mark.png logo-full.png dist/

CLOUDFLARE_API_TOKEN=$(cat ~/.config/ipda/cloudflare-911511) \
CLOUDFLARE_ACCOUNT_ID=2520726af8a73ddd60097700ebec6562 \
npx wrangler deploy
```

`wrangler.jsonc` holds the worker name, the `dist` assets directory, `workers_dev`
(staging URL) and the two custom-domain routes, so the domain stays attached on
every deploy.

## Notes

- `_headers` works with Workers static assets; it sets HSTS, CSP, nosniff,
  frame options and long-lived caching for `logo-*.png`.
- The CSP allows `cdn.tailwindcss.com` (the page still uses the CDN build),
  `fonts.googleapis.com` / `fonts.gstatic.com` and Cloudflare Web Analytics.
  If the CSS is ever compiled locally, drop `cdn.tailwindcss.com` from the policy.
- Mail records (`MX`, SPF, DMARC, autodiscover) were intentionally kept in the
  zone; only the GoDaddy parking `A` records and the old `www` CNAME were removed.
- Tokens used for deploys live in `~/.config/ipda/` (mode 600). Revoke them at
  <https://dash.cloudflare.com/profile/api-tokens> if the machine is retired.
