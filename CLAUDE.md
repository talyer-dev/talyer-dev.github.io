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

Landing page for talyer.dev — an AI and data science consultancy in Manila,
run by four friends who play Dota together. Businesses bring us a problem
(forecasting, inventory/waste, customer insights, search/recommendation,
manual data work) and we build the fix. The site leans into:
- "Talyer" = Filipino for a neighborhood auto-repair workshop. The metaphor:
  a shop you bring a broken problem to, except our tools are data and AI
- Dota culture: the crew keeps positions 1–5. Joshua (position 1, carry) is
  the founder/data scientist who does the actual work; positions 3–5 are "the
  minions" (funny framing, but competent support/ops); position 2 (mid) is
  reserved for "your problem", a client-facing twist on the old open-slot joke
- Honest-new-shop positioning: we're new and hungry and say so as a trust
  signal. No fake logos, ratings, or invented track record — one live demo
  (komikfind) is the proof of capability instead

Tone for all copy: witty, honest, Filipino + Dota in-jokes, but every section
must make a potential CLIENT think "these people could solve my problem."
Never corporate. If a line could appear on a generic SaaS template, rewrite
it. No em dashes in site copy (removed site-wide — keep it that way).

## Stack & structure

- Single static `index.html` — all CSS and JS inline. No build step, no
  framework. Keep it that way unless there's a strong reason.
- One runtime dependency: three.js r128 from the cdnjs CDN, lazy-loaded after
  `load` (via requestIdleCallback) for the scroll-driven 3D backdrop of shop
  hardware (hex nuts, bolts, washers, gears). It is skipped entirely under
  prefers-reduced-motion or if WebGL init fails — the static painted
  background is the fallback, so the page must always work without it.
  r128 constraints: no OrbitControls, no CapsuleGeometry. Pixel ratio is
  capped, rendering pauses when the tab is hidden, and mobile gets fewer
  meshes — don't regress these.
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

1. ~~Email capture form (waitlist via Formspree).~~ Replaced — the site is now
   client-facing, so the CTA is "bring us your problem": a `mailto:` link to
   joshuac@talyer.dev with a prefilled subject. The old Formspree waitlist
   form and its JS were removed with the repositioning. If a proper contact
   form comes back, point its delivery at joshuac@talyer.dev.
2. Crew cards: Joshua (pos 1) is named; positions 3–5 are still personas
   (The Operator, The Closer, The Glue). Replace with real names/handles
   and personalized roasts when the team decides how identifiable they
   want to be.
3. ~~Add favicon + Open Graph/social preview image.~~ Done — favicon set,
   OG/Twitter tags, and social image shipped (see `/assets`).
4. Optional: self-host fonts to remove the Google Fonts dependency.
5. ~~Future: a "Now Serving" / portfolio section.~~ Done — the "Now serving"
   section ("Work orders") sits after "What we fix" in `index.html`. Each
   shipped project is one `.workorder` card; to add another, duplicate the
   `<article class="workorder">` block (a comment marks the spot) and bump
   the JOB number. The HUD shipped-count is derived from the number of
   `.workorder` cards, so it updates automatically. First entry is komikfind
   (JOB #001, live at `komikfind.talyer.dev`), framed as the live demo that
   proves we build ML/semantic-search systems — keep that client-facing
   framing. Once there are several cards, this graduates to its own `/work`
   page. Every portfolio page that ships (not the cards, but any standalone
   page) MUST be added to `sitemap.xml`.

## Conventions

- One HTML file until complexity demands otherwise. If splitting becomes
  necessary, plain CSS/JS files — still no build step.
- Mobile-first checks: the page must look right at 380px width.
- Accessibility floor: visible focus states, labels on inputs, role/status
  attributes on dynamic messages. These exist — don't regress them.
- Commit messages: short imperative, e.g. "Add OG tags", "Wire email form".
