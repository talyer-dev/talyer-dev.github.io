# CLAUDE.md — talyer.dev

Behavioral guidelines to reduce common LLM coding mistakes, followed by
project-specific instructions for this repo.

**Tradeoff:** These guidelines bias toward caution over speed. For trivial
tasks, use judgment.

## 1. Think Before Coding

**Don't assume. Don't hide confusion. Surface tradeoffs.**

Before implementing:
- State your assumptions explicitly. If uncertain, ask.
- If multiple interpretations exist, present them — don't pick silently.
- If a simpler approach exists, say so. Push back when warranted.
- If something is unclear, stop. Name what's confusing. Ask.

## 2. Simplicity First

**Minimum code that solves the problem. Nothing speculative.**

- No features beyond what was asked.
- No abstractions for single-use code.
- No "flexibility" or "configurability" that wasn't requested.
- No error handling for impossible scenarios.
- If you write 200 lines and it could be 50, rewrite it.

Ask yourself: "Would a senior engineer say this is overcomplicated?"
If yes, simplify. This repo is a single static HTML file on purpose —
that bar applies doubly here.

## 3. Surgical Changes

**Touch only what you must. Clean up only your own mess.**

When editing existing code:
- Don't "improve" adjacent code, comments, or formatting.
- Don't refactor things that aren't broken.
- Match existing style, even if you'd do it differently.
- If you notice unrelated dead code, mention it — don't delete it.

When your changes create orphans:
- Remove CSS rules/JS functions that YOUR changes made unused.
- Don't remove pre-existing dead code unless asked.

The test: every changed line should trace directly to the user's request.

## 4. Goal-Driven Execution

**Define success criteria. Loop until verified.**

Transform tasks into verifiable goals:
- "Fix the layout" → "Verify the page renders correctly at 380px width"
- "Add OG tags" → "Confirm tags appear in <head> and validate the preview"
- "Refactor X" → "Confirm the page looks and behaves the same before and after"

For multi-step tasks, state a brief plan:
```
1. [Step] → verify: [check]
2. [Step] → verify: [check]
3. [Step] → verify: [check]
```

Strong success criteria let you loop independently. Weak criteria
("make it work") require constant clarification.

---

## What this project is

Landing page for talyer.dev — a pre-product startup by four friends in Manila
whose only common ground (so far) is playing Dota together. The site is a witty
"coming soon" page that leans into:
- "Talyer" = Filipino for a neighborhood auto-repair workshop
- Dota culture: the four founders are presented across positions 1–5 (carry,
  offlane, roamer, hard support), with position 2 (mid) as an open-slot
  recruiting joke
- Self-aware honesty: the product/market/business model are openly listed as TBD

Tone for all copy: witty, self-deprecating, Filipino + Dota in-jokes. Never
corporate. If a line could appear on a generic SaaS template, rewrite it.

## Stack & structure

- Single static `index.html` — all CSS and JS inline. No build step, no
  framework, no dependencies. Keep it that way unless there's a strong reason.
- Fonts via Google Fonts: Anton (display), Space Grotesk (body),
  IBM Plex Mono (HUD/labels). Fallbacks are defined; don't remove them.
- `CNAME` file in repo root contains `talyer.dev`. NEVER delete or modify it —
  GitHub Pages custom domain breaks without it. If you regenerate or move
  files, preserve it.
- `/assets` — branding images, all derived from `logo-base.png` (the source;
  regenerate the others from it if the logo changes): `logo.png` (full badge,
  transparent), `logo-mark.png` (hex-bolt mark for small sizes), `og-image.png`
  (1200×630 social preview), `favicon-32.png`, `apple-touch-icon.png`.
  `favicon.ico` lives at repo root for legacy auto-discovery.
- `robots.txt` and `sitemap.xml` at repo root. The sitemap lists every page
  on the site — currently just `/`.

## Design system (use these, don't invent new tokens)

- --steel #1B232E / --steel-deep #141A22 — corrugated night-steel background
- --enamel #F2EBDA — sign paint (light panels, founder cards)
- --sign-red #C8372B, --sign-yellow #F2B33D, --sign-blue #3D7BC4 — accents
- --ink #1A1410 — hand-painted lettering on enamel
- Aesthetic: hand-painted Filipino shop signage (bolts, tilted sign, painted
  borders), match-HUD monospace overlays. Cards have hard ink borders and
  offset solid shadows, not soft blurs.
- Respect prefers-reduced-motion (already implemented — keep it working).

## Deployment

- GitHub Pages, org repo: talyer-dev/talyer-dev.github.io, branch `main`,
  root folder. Push to main = deploy. No CI needed.
- Custom domain talyer.dev. DNS on Cloudflare (registrar + DNS), records MUST
  stay "DNS only" (grey cloud, not proxied) or GitHub's TLS cert breaks.
- .dev TLD is HSTS-preloaded: the site only works over HTTPS. If the site
  appears down, check certificate status in repo Settings → Pages first.

## Current TODOs

1. Email capture form is front-end only (shows a fake success message).
   Wire it to a real list — Buttondown, Mailchimp, a Google Form, or a
   Cloudflare Worker + KV. Keep the "GG — you're in the lobby" success copy.
2. Founder cards use placeholder personas (The Builder, The Operator,
   The Closer, The Glue, plus the pos-2 open slot). Replace with real
   names/handles and personalized roasts when the team decides how
   identifiable they want to be.
3. ~~Add favicon + Open Graph/social preview image.~~ Done — favicon set,
   OG/Twitter tags, and social image shipped (see `/assets`).
4. Optional: self-host fonts to remove the Google Fonts dependency.
5. Future: a "Now Serving" / portfolio section listing small apps we ship,
   each as a work-order card in the talyer aesthetic. Not yet — just planned.
   Every portfolio page that ships MUST be added to `sitemap.xml`.

## Conventions

- One HTML file until complexity demands otherwise. If splitting becomes
  necessary, plain CSS/JS files — still no build step.
- Mobile-first checks: the page must look right at 380px width.
- Accessibility floor: visible focus states, labels on inputs, role/status
  attributes on dynamic messages. These exist — don't regress them.
- Commit messages: short imperative, e.g. "Add OG tags", "Wire email form".
