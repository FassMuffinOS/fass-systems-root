# Archive: what fass.systems was before this umbrella page replaced it

Captured 2026-07-01, before moving the `fass.systems` domain from the
`fass-landing` Vercel project to this one. Nothing was deleted — the
`fass-landing` repo, its Git history, and its GitHub remote
(`FassMuffinOS/fass-landing`) are untouched. This file just preserves the
pitch/copy and a map of what was built, so the concept isn't lost even
though it's no longer the front door.

## What was actually live at fass.systems (fetched 2026-07-01)

> **FASS — The OS for Autonomous Agents**
> Deploy your first autonomous agent in seconds.
>
> DUAL-AGENT ARCHITECTURE · DEEPSEEK BACKEND · AUTHORITY ENVELOPES
>
> FASS fuses the observability of Datadog with the execution power of AI
> agents. Francis navigates. Forge executes. You ship.
>
> **4-Layer Forge Architecture**
> 1. Planner — Decomposes goals into executable task graphs using DeepSeek reasoning.
> 2. Operator — Selects and invokes tools — GitHub, Stripe, Vercel, Cloudflare.
> 3. Memory — Vector store + episodic context. Forge remembers across sessions.
> 4. Verifier — Authority Envelopes enforce provable, auditable trust boundaries.
>
> Included a live "developer sandbox" terminal that triggered a real Stripe
> test-mode API call on `fass deploy`, and a status link out to
> `monitor.fass.systems`.

## What the repo had actually pivoted to since (not yet deployed)

The local `fass-landing` repo's most recent commit ("Add Phase 1 public
landing routes and CTA paths") had already moved on to a *different* framing
than what was live — an institutional research/publishing concept:

> **Francis Research Bureau · The Nobles Machine**
> Turn scattered work into governed institutional knowledge.
>
> Four-Pillar Architecture: Solutions, Practices, Principles, Understanding.
> Pipeline: Ingest → Synthesize → Witness → Publish.

So there were at least two distinct large pivots on this domain (an
"AI agent OS" product, then an "institutional research bureau" product),
neither of which was FASS Flow or Regulars — the two products that actually
have paying-customer infrastructure behind them.

## Scope of what was built (for reference, not action)

Real engineering went into this — dozens of Next.js routes existed:
`/`, `/company`, `/company/[slug]`, `/contact`, `/francis`, `/investors`,
`/media`, `/methods`, `/methods/[slug]`, `/operators`, `/practices`,
`/practices/[slug]`, `/principles`, `/principles/[slug]`, `/publications`,
`/publications/[series]`, `/publications/[series]/[slug]`, `/research`,
`/research/[slug]`, `/resources`, `/resources/[slug]`, `/signin`,
`/solutions`, `/solutions/[slug]`, `/trust`, `/trust/[slug]`,
`/understanding`, `/understanding/[slug]`, plus an `/app` (protected) area
and `/auth/status`.

Dependencies included real integrations, not just static pages: `stripe`,
`@upstash/ratelimit` + `@upstash/redis`, `@xterm/xterm` (the terminal demo),
`framer-motion`.

## Where to find it if any of this is worth revisiting later

- Local: `fass-landing/` (sibling folder to this one)
- GitHub: `FassMuffinOS/fass-landing`
- Security note: this repo's local `.git/config` had a GitHub token embedded
  directly in the remote URL (`gho_...`). Worth rotating that token in
  GitHub → Settings → Developer settings → regardless of what happens to
  this repo, since it's sitting in plaintext on disk.
