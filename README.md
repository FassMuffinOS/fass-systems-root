# fass-systems-root

## 2026-07-05: fixed support.fass.systems intermittently showing the wrong homepage

`fass.systems`, `www.fass.systems`, and `support.fass.systems` are all
custom domains on this *same* Vercel project/deployment. The `redirects`
rule in `vercel.json` sends `support.fass.systems`'s `/` to `/support.html`
based on the `Host` header — that rule itself was correct. But repeated
fetches of `https://support.fass.systems/` showed the *general* homepage
content roughly as often as the support page, with no code change in
between. Appending a cache-busting query string (`?cb=random`) made the
redirect fire correctly every single time.

That's the signature of a caching layer keying its cache by path only,
ignoring the `Host` header — plausible here specifically because multiple
domains share one deployment, so a `/` response cached from a `fass.
systems` visit can get served back to a `support.fass.systems` visitor (or
vice versa) depending which edge cache entry a given request happens to
hit. Fixed by adding an explicit `Cache-Control: no-store, must-revalidate`
header on the `/` route in `vercel.json`, forcing every request to
re-evaluate the host-conditional redirect instead of risking a stale,
wrong-domain cache hit. Re-deploy and re-verify with a few uncached fetches
before considering this fully closed — caching bugs like this can hide
again if a future change reintroduces aggressive caching on this route.


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

## Deploy it

Checked live via the Vercel API on 2026-07-01 — correcting an earlier wrong
guess in this README: `fass.systems` is **not** attached to
`mercury-portfolio` (that project's last deploy actually errored, it isn't
live). The domain is attached to **`fass-landing`**, a fully-built Next.js
app — see `ARCHIVE_fass-landing.md` in this repo for what that was serving
before this replaces it.

**Steps:**
1. Push this folder to a new GitHub repo (e.g. `FassMuffinOS/fass-systems-root`,
   matching the naming convention the other repos use).
2. In the Vercel dashboard (team: FASS Technologies LLC): **Add New → Project**
   → import that repo → Framework Preset: "Other" (no build command needed,
   it's a static `index.html`) → Deploy.
3. On the new project: **Settings → Domains → Add** → type `fass.systems`.
   Vercel will detect it's already assigned to `fass-landing` and offer to
   **move it** — one click, no DNS change needed (Cloudflare's `fass.systems`
   A record already points at Vercel's `76.76.21.21`, DNS-only, so it's
   correct for any Vercel project). Do the same for `www.fass.systems` if it's
   attached anywhere.
4. Confirm at `https://fass.systems` — should show the new page within a
   minute or two.

I don't have a way to run this deploy or move the domain myself (no CLI in
my sandbox, no domain-management tool in the Vercel connector I have — just
read access to list projects/deployments). This part needs your hands.

Once confirmed live, `mercury-portfolio` (errored, not live, not attached to
any real domain) and `fass-landing` (now detached from `fass.systems`, still
reachable at its `.vercel.app` URL) both become fair game for the cleanup
list below.

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

**Vercel projects** (Vercel dashboard → project → Settings → Delete) — full
list confirmed live via the Vercel API on 2026-07-01, 11 projects total:
- [ ] `fass-landing` — only after `fass.systems` domain is moved off it (see
      Deploy steps above). Real Next.js app, archived in
      `ARCHIVE_fass-landing.md` first.
- [ ] `mercury-portfolio` — already not live (last deploy errored), not
      attached to any real domain. Safe to delete any time.
- [ ] `fass-control-plane` (control.fass.systems) — dormant restaurant-ops
      side project.
- [ ] `fass-menu-architect` (spiceroute.fass.systems) — same restaurant-ops
      side project, different piece.
- [ ] `spiceroute` — orphaned, no git repo connected.
- [ ] `fass-forge-video-mockup` — one-off mockup.
- [ ] `fass-app-dashboard` — unreviewed, confirm before deleting.
- [ ] `fass-app-docs` (docs.fass.systems) — confirm this is actually dead
      before removing; "docs" sounds like it could still be linked from
      somewhere.
- [ ] `axiom-fass-systems` and `axiom-fass-systems-updated-3-6-2026` — two
      versions of the same project exist; confirm which (if either) is
      current before deleting the other.

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
