# talyer.dev

Landing page for **talyer.dev** — five founders, zero product, one shared
calendar event: Dota night.

> **talyer** /tal·yér/ *n., Filipino* — a workshop where things get taken
> apart, figured out, and fixed. Eventually.

## Status

🟢 The shop is open. 🟡 The product is still in the drafting phase.

## Stack

One `index.html`. That's it. No framework, no build step, no node_modules
folder heavier than our ambitions.

- Hosted on **GitHub Pages** (this repo, `main` branch, root)
- Domain: **talyer.dev** (Cloudflare registrar, DNS-only records → GitHub Pages)
- `CNAME` file = custom domain config. Do not delete it. Seriously.

## Local development

```bash
# the entire dev environment:
open index.html        # macOS
xdg-open index.html    # Linux
start index.html       # Windows
```

Or `python3 -m http.server` if you want to feel professional.

## Deploying

```bash
git add . && git commit -m "your change" && git push
```

GitHub Pages redeploys automatically in ~1 minute.

## Contributing

You're one of five people on Earth with write access. You know who you are.
Ping the group chat before redesigning anything during Dota night.
