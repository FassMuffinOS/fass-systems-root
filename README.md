# fass-systems-root

The new umbrella page for `fass.systems` — promotes FASS Flow and Regulars,
frames FASS Technologies as a growing product company. Single static
`index.html`, no build step, no framework. Deliberately simple so it ships
today instead of becoming its own project.

**Note:** this is a brand-new, from-scratch repo. It is NOT the same as the
old `fass.systems` folder already on this machine (GitHub remote
`FassMuffinOS/fass.systems`) — that repo is an abandoned "deterministic
authority / governance / Kernel.ts" experiment, explicitly marked
`SYSTEM_ROLE: EXPERIMENTAL_EXECUTION_SANDBOX` / `AUTHORITY: NON_CANON` with
`DO_NOT_RUN_FROM_ROOT` warnings baked into it. Don't deploy that one — this
one is the real, working replacement.

## Deploy it (pick one)

**Option A — new Vercel project (recommended, cleanest):**
1. Push this folder to a new GitHub repo (e.g. `fass-systems-root`).
2. In Vercel: New Project → import that repo → Framework Preset: "Other" →
   deploy (no build command needed, it's a static `index.html`).
3. Vercel Project Settings → Domains → add `fass.systems` and `www.fass.systems`.
4. Vercel will tell you the DNS is already correct (Cloudflare's `fass.systems`
   A record already points at Vercel's `76.76.21.21`, DNS-only) — no DNS
   change needed, just remove `fass.systems` from the **mercury-portfolio**
   project's domains first (a domain can only be attached to one Vercel
   project at a time) and add it here.

**Option B — replace mercury-portfolio's content directly:**
1. Open the `mercury-portfolio` repo, replace its content with this
   `index.html`, commit, push. It's already the project `fass.systems` is
   attached to, so nothing to reconfigure in Vercel.
2. Simpler, but permanently overwrites the old portfolio history in that repo
   — use Option A instead if you want to keep mercury-portfolio intact
   somewhere.

## Third-party cleanup checklist (do one at a time)

**Cloudflare Workers** (Workers & Pages → click into each → Delete):
- [ ] `fass-root` — confirmed old experiment, not in use, not proxied. Delete.
- [ ] `fass-atlas` — pairs with fass-root (a JWT "authority" gate), live at
      `atlas.fass.systems` (Proxied). Delete the Worker, then delete the
      `atlas.fass.systems` DNS record too or it'll dangle.
- [ ] `fass-monitor` — live at `monitor.fass.systems` (Proxied), but it's a
      self-contained demo/mockup "observability dashboard" with fabricated
      log lines, not connected to real infrastructure. Safe to delete along
      with the `monitor.fass.systems` DNS record.
- [ ] `fass-music` — bound to `audio.fass.systems` per DNS (Proxied). Confirm
      you don't need it, then delete Worker + that DNS record.
- [ ] `fass-audio` — no DNS record pointed at it in what's been reviewed so
      far (possibly orphaned already). Confirm, then delete.

No D1 databases, no KV namespaces, and R2 isn't enabled on this Cloudflare
account — so none of the above deletions can silently lose stored data.

**Cloudflare DNS** (after the Workers above are gone):
- [ ] Remove `atlas.fass.systems`, `monitor.fass.systems`, `audio.fass.systems`
      records once their Workers are deleted (otherwise they 404/error but
      still show as "configured").
- [ ] `api.fass.systems` → raw A record to `206.189.200.7` (not Railway) —
      separate from the real backend. Confirm what this is before touching
      it; if it's dead, remove the record.

**Vercel projects** (Vercel dashboard → project → Settings → Delete):
- [ ] `mercury-portfolio` — only after `fass.systems` domain is moved to the
      new project above.
- [ ] `fass-control-plane` (control.fass.systems) — dormant restaurant-ops
      side project.
- [ ] `fass-menu-architect` (spiceroute.fass.systems) — same restaurant-ops
      side project, different piece.
- [ ] `spiceroute` — orphaned, no git repo connected.
- [ ] `fass-forge-video-mockup` — one-off mockup.
- [ ] `axiom-fass-...` — unreviewed, confirm before deleting.

**Not touched, and not recommended to touch right now:**
- The old `fass.systems` GitHub repo (`FassMuffinOS/fass.systems`) — leave it
  archived as-is. It's not wired to anything live and re-opening it risks
  pulling focus into an abandoned concept instead of shipping the real
  product.
- ~40 other local `fass-*` folders on this machine (fass-hub, fass-engine,
  fass-witness-system, fass-authority, fass-ai-controller, etc.) — these
  surfaced while looking for this repo and were not individually reviewed.
  Worth a separate, dedicated triage pass later; out of scope for "get this
  version out."
