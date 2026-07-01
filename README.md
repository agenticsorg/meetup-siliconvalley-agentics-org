# Silicon Valley Agentics Foundation Meetup

## What this is

Landing page for the **Silicon Valley Agentics Foundation Meetup** — **Thursday, July 23, 2026,
6:00–9:00 PM PDT, at Plug and Play Tech Center, Sunnyvale, CA.** Registration is handled by Luma
(**https://luma.com/agentics-silicon-valley**); this page exists to drive traffic and RSVPs to it.

## Stack

Plain static **HTML/CSS — zero build step, zero dependencies.** `index.html` *is* the site (all
CSS inlined; only Google Fonts load over the network). Everything else is static assets you can
serve from any static host.

The original design source is preserved in the gitignored **`_design/`** folder (and in `main`'s
history). `index.html` is a port of it — if the design changes, update `_design/` and re-port;
don't redesign in place.

```
index.html          The site (deliverable)
img/                Images — originals (PNG/JPG) + optimized .webp variants + logo-af.svg + og-image.png
favicon.svg         Coral "›" on cream
robots.txt          Allow all + sitemap pointer
sitemap.xml         Single-URL sitemap
404.html            On-brand not-found page
CNAME               Custom domain for GitHub Pages (meetup.agentics.org)
.github/workflows/pages.yml   Static deploy-on-push-to-main (inert until Pages is enabled)
_design/            Pristine design source of truth (GITIGNORED — not deployed)
```

## Local preview

```bash
python3 -m http.server 8080
# open http://localhost:8080
```

## GO-LIVE CHECKLIST

- [ ] Merge PR #1 to `main`
- [ ] Repo Settings → Pages → Source: **GitHub Actions** (the `pages.yml` workflow deploys on push to main)
- [ ] Verify at https://agenticsorg.github.io/meetup-siliconvalley-agentics-org/
- [ ] DNS: add CNAME record `meetup` → `agenticsorg.github.io` on agentics.org (whoever holds DNS)
- [ ] Settings → Pages → Custom domain: `meetup.agentics.org` → wait for check → Enforce HTTPS
- [ ] Smoke-test https://meetup.agentics.org/ — images, 3 Join CTAs (Luma, new tab), Maps link, view-source schema
- [ ] Validate https://search.google.com/test/rich-results (Event + FAQPage should both pass)
- [ ] Add property + request indexing in Google Search Console
- [ ] Paste URL in Slack/LinkedIn to confirm the OG share card renders
- [ ] Point ad/social traffic at https://meetup.agentics.org/
- [ ] (Separate repo) agentics-org-v2 nav: Connect → Events → "Silicon Valley Meetup" → external link to https://meetup.agentics.org/ (new tab)

> **Note:** merging the PR alone does **not** publish the site — GitHub Pages must be enabled in
> Settings (checklist step 2). The custom domain `meetup.agentics.org` won't resolve until the DNS
> CNAME record exists.

## Contact

**Mat Mathews** — mat@agent86.ai — page owner.
