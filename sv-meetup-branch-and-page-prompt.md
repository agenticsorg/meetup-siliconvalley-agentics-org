# Claude Code Prompt — SV Meetup Landing Page on a NEW BRANCH + Connect nav link

> Run in `agentics-org-v2`. **Do not deploy. Do not push. Do not commit until reviewed.**
> This supersedes the earlier "hidden page" version of this prompt — the page is now LINKED
> in the nav under **Connect**. The finished HTML design is provided alongside this prompt as
> `silicon-valley-meetup.html` with an `img/` folder — treat them as the design source of truth.
> Port, don't redesign.

---

## STEP 0 — Create the branch
The new nav (Build · Learn · Connect · Hire · Support) lives on `mat/nav-mega-menu`, and this task
edits the Connect panel — so branch off **that**, not `content-update`:
```
git status                                  # if there's uncommitted work, STOP and tell me —
                                            # Mat decides commit vs stash before switching anything
git checkout mat/nav-mega-menu
git checkout -b mat/silicon-valley-meetup
```
- Sanity-check the base: `grep -rn "Connect" <nav component>` should show the new mega-menu Connect
  panel. If it doesn't, STOP and report — wrong base.
- All work in this session happens on `mat/silicon-valley-meetup`.
- **Merge-order implication (for the runbook):** `mat/nav-mega-menu` must merge before this branch,
  since this branch contains all of its commits plus the meetup page.

## GOAL
1. Create the landing page at **`/events/silicon-valley-meetup/`** for the Silicon Valley Agentics
   Foundation Meetup (Thu Jul 23, 2026 · 6–9 PM PDT · Plug and Play Tech Center, Sunnyvale).
2. Link it in the main nav: **Connect → Events → Silicon Valley Meetup** (details in Step 3).
3. Indexable: `index, follow`, canonical set, in the sitemap if one is generated. NO noindex.
4. All CTAs → `https://luma.com/agentics-silicon-valley` in a new tab (`rel="noopener noreferrer"`).
   Luma handles registration — the page holds zero form state.

## GUARDRAILS
- Branch: `mat/silicon-valley-meetup` only. **Never deploy. Never push to main. No commits until Mat reviews.**
- Touch ONLY: (a) the new route + its assets, (b) the shared nav component — and in the nav,
  ONLY the Connect panel. Nothing else on any page.
- The page ships its own scoped styles (Archivo + IBM Plex Mono, dark hero, cream body). Keep it
  **self-contained** — don't merge its styles into global CSS, don't let global styles break it.
  Use a bare/minimal layout: the page has its own header + footer by design (it's a conversion
  landing page — keep it that way even though it's nav-linked; its logo links back to the homepage).
- **Keep both JSON-LD blocks verbatim** (Event + FAQPage) and the full `<head>` meta set. Adjust
  canonical/og:url only if the final route differs.
- Brand tokens: cream `#FAF9F6`, coral `#FF6847`, `#1A1A1A`, `#6B7280` — already baked in.

## PROVIDED ASSETS (`img/` folder — copy into the repo)
| File | Used in | Notes |
| --- | --- | --- |
| `badge.png` | Hero — floating event-badge card | `fetchpriority="high"` |
| `ruv-stage.jpg` | Founder quote band | `loading="lazy"` |
| `aerial-plugandplay.jpg` | Venue section | `loading="lazy"` |
| `plugandplay-logo.png` | Venue "official venue" chip | small wordmark |
| `og-image.png` | `og:image` meta only (1200×630) | not rendered on the page |

Destination: `public/events/silicon-valley-meetup/img/` — then update the page's `src` and
`og:image` paths from relative `img/...` to `/events/silicon-valley-meetup/img/...`.

## STEP 1 — Inventory (confirm before editing)
```
ls src/pages 2>/dev/null; ls src/pages/events 2>/dev/null      # does /events/ exist?
grep -rli "nav\|header\|navbar" src/components 2>/dev/null      # the shared nav component
grep -rn "Connect" <nav component file>                          # current Connect panel items
grep -rli "sitemap" astro.config.* src/ 2>/dev/null              # sitemap generation?
git log --oneline -5 -- <nav component file>                     # recent nav changes (Ofer?)
```
Report: whether `/events/` exists, the nav component path, the **current Connect panel contents**,
whether there are recent/unmerged nav changes that look like someone else's Events work, and the
sitemap setup. **Confirm with me before editing.**

## STEP 2 — Build the page
1. Create the route (e.g. `src/pages/events/silicon-valley-meetup.astro`).
2. Copy the `img/` assets to `public/events/silicon-valley-meetup/img/`; update image paths + og:image.
3. Port `silicon-valley-meetup.html`: head meta + both JSON-LD scripts + font links into the page
   head; body sections as-is, in a bare layout. **Keep alt text, width/height, `loading="lazy"`,
   and `fetchpriority="high"` exactly as provided** (SEO + CLS prevention).
4. **Image optimization:** `badge.png` is ~500KB. If the repo has an image pipeline (Astro `<Image>`,
   sharp), run the three photos through it (WebP, ~80 quality, max 1400px). If not, convert to
   compressed WebP with `<img>` fallback — or flag it and leave originals. Leave `og-image.png` as PNG.
5. **Logos:** swap the text "AGENTICS [FOUNDATION]" wordmark in the page header for the site's real
   logo asset if one exists (confirm path first). Keep `× COGNITUM ONE` as the mono wordmark — no
   Cognitum logo asset exists yet; flag where to swap it later.

## STEP 3 — Nav: Connect → Events → Silicon Valley Meetup
The Connect panel currently holds **Community Home** and **Calendar**. Add an **Events** grouping:

- **If the Connect panel supports sub-groups/headers:** add a small mono group label `EVENTS`
  beneath the existing items, with one link under it: **Silicon Valley Meetup** → `/events/silicon-valley-meetup/`.
- **If the panel is a flat list:** add a single item **Silicon Valley Meetup** directly after
  Calendar (no sub-group), and note that in your report.
- Style it exactly like the existing panel items (hover, focus, mobile accordion). Internal link,
  same tab.
- **Do NOT create an `/events/` index page** — Ofer may be publishing an Events section separately.
  If you find an existing or in-progress Events nav entry or `/events/` index during Step 1, STOP
  and show me before touching the nav — we'll slot our link into his structure instead of creating
  a parallel one.
- No other nav changes. Every other panel and item stays byte-identical.

## STEP 4 — Sitemap
If the site auto-generates a sitemap, confirm the new route is included. If manual, add it.

## STEP 5 — Verify
- `npm run dev` → `/events/silicon-valley-meetup/`:
  - Hero split (copy left, tilted badge card right; badge stacks below on mobile).
  - Founder quote band (Ruv photo + quote), project cards, hosts, venue photo + PnP logo chip, FAQ.
  - All images load from `/events/silicon-valley-meetup/img/` — no 404s.
  - 3 "Join" CTAs (header, hero, closing) open the Luma URL in a new tab; Maps link works.
  - View source: both JSON-LD blocks, title/description/canonical, absolute og:image path, no noindex.
- Nav: desktop Connect panel shows the new Events → Silicon Valley Meetup link and it resolves;
  mobile accordion shows it; every other nav item unchanged; header still renders on all pages.
- Take screenshots: the landing page (desktop + mobile) and the open Connect panel.
- Print: files changed, nav approach used (sub-group vs flat), image optimization before → after,
  anything that looked like Ofer's Events work, sitemap status.
- **Do not commit. Do not push. Wait for my review.**

---

## GO-LIVE RUNBOOK (Mat — manual, after reviewing Claude Code's output)
1. **Review** the dev-server output + screenshots. Request fixes on the branch if needed.
2. **Commit & push the branch:** `git add -A && git commit -m "feat: Silicon Valley meetup landing page + Connect nav link" && git push -u origin mat/silicon-valley-meetup`
3. **Open PRs in order:** `mat/nav-mega-menu` → main first (this branch is built on top of it).
   Once that merges, open the PR for `mat/silicon-valley-meetup` → main (it will then show only
   the meetup-page + Connect-link changes): `gh pr create --fill`
4. **Coordinate with Ofer BEFORE merging** — if his Events work touches the nav, decide merge order
   to avoid a conflict (smaller change merges second and rebases).
5. **Merge the PR.** Cloud Run deploys from main per the existing pipeline.
6. **Bump the submodule pointer** in the `dashboard` monorepo (agentics-org-v2 is a submodule —
   the live site won't pick up the merge until the pointer moves):
   `cd dashboard && git submodule update --remote agentics-org-v2 && git add agentics-org-v2 && git commit -m "chore: bump agentics-org-v2 (SV meetup page)" && git push`
7. **Smoke-test production:** https://agentics.org/events/silicon-valley-meetup/ — images, CTAs,
   nav link, view-source for schema.
8. **SEO activation:** validate at https://search.google.com/test/rich-results (Event + FAQ pass) →
   request indexing in Google Search Console → paste the URL in Slack/LinkedIn to confirm the OG card.
9. **Drive traffic** to `https://agentics.org/events/silicon-valley-meetup/`.
