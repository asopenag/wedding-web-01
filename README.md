# wedding-web-01

Wedding website built with VitePress + Tailwind 4.

🌐 **Live site:** https://asopenag.github.io/wedding-web-01/

## Structure

- `pages/` — source content (config.json + markdown pages)
- `docs/.vitepress/` — VitePress config and Vue components
- `docs/public/` — static assets (images, media)

## Development

```bash
npm install
npm run docs:before-build   # generate style.css, manifest, process pages
npm run docs:dev             # dev server
npm run docs:build           # production build
```

See [AGENTS.md](./AGENTS.md) for full architecture and block system docs.
