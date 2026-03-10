# AGENTS.md — wedding-web-01

This is a VitePress + Tailwind 4 wedding site. Source content lives in `pages/`, generated output in `docs/`.

## Architecture

- `pages/config.json` — site-wide config (theme, languages, wedding date, metadata)
- `pages/*.md` — source pages with YAML frontmatter; `sections[]` drive the block-based layout
- `docs/.vitepress/config.js` — VitePress config (reads from pages/config.json)
- `docs/.vitepress/theme/components/` — Vue components, one per block type
- `docs/.vitepress/theme/Layout.vue` — main layout, iterates sections and renders blocks
- `docs/public/` — static assets (images, media)
- `.github/workflows/deploy.yml` — CI/CD to GitHub Pages

## Block System

Each page section has a `_block` field that maps to a Vue component:

| `_block` value | Component | Purpose |
|---|---|---|
| `hero` | `Hero.vue` | Full-bleed hero image with title and CTA buttons |
| `countdown` | `Countdown.vue` | Live countdown timer to wedding date |
| `gallery` | `Gallery.vue` | Multi-purpose: text (`type: text`), image grids, timelines, etc. |
| `map` | `Map.vue` | Leaflet map using `geo: "lat, lng"` field |
| `accordion` | `Accordion.vue` | Expandable FAQ sections |
| `spotify` | `Spotify.vue` | Spotify playlist embed |
| `rsvp` | `Rsvp.vue` | RSVP mailto form |
| `registry` | `Registry.vue` | Gift registry / IBAN display |
| `contacts` | `Contacts.vue` | Bride & groom contact cards |

To add a new block type: create `ComponentName.vue` in `docs/.vitepress/theme/components/` and use `_block: component-name` in the markdown frontmatter.

## Development Workflow

```bash
cd /home/alex/.openclaw/workspace/wedding-web-01
npm install
npm run docs:before-build   # generates style.css, manifest, processes pages/
npm run docs:dev             # dev server
npm run docs:build           # production build
```

## Agent Guidelines

- Source content is in `pages/` — that's where you edit text, sections, images
- Components are in `docs/.vitepress/theme/components/` — that's where you add/fix UI
- Always test `npm run docs:before-build` before committing
- Use `git commit --author="Greg <greg.assistant.91@gmail.com>"`
- Branch: `main`
- Deploy: automatic on push to main (GitHub Pages via GitHub Actions)

## Known Issues / History

- Initially based on the parish website template (asopenag org)
- Extended with wedding-specific blocks: Countdown, Rsvp, Spotify, Registry, Contacts
- Uses the same block system as the parish template for consistency
