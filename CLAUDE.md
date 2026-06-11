# CLAUDE.md — talyer.dev

## What this is
Landing page for talyer.dev — a pre-product startup by five friends in Manila
whose only common ground (so far) is playing Dota together. The site is a witty
"coming soon" page that leans into:
- "Talyer" = Filipino for a neighborhood auto-repair workshop
- Dota culture: the five founders are presented as positions 1–5 (carry,
  mid, offlane, roamer, hard support)
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
2. Founder cards use placeholder personas (The Builder, The Visionary,
   The Operator, The Closer, The Glue). Replace with real names/handles and
   personalized roasts when the team decides how identifiable they want to be.
3. Add favicon + Open Graph/social preview image (a version of the painted
   sign would be ideal). OG tags not yet present in <head>.
4. Optional: self-host fonts to remove the Google Fonts dependency.

## Conventions
- One HTML file until complexity demands otherwise. If splitting becomes
  necessary, plain CSS/JS files — still no build step.
- Mobile-first checks: the page must look right at 380px width.
- Accessibility floor: visible focus states, labels on inputs, role/status
  attributes on dynamic messages. These exist — don't regress them.
- Commit messages: short imperative, e.g. "Add OG tags", "Wire email form".
