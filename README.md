# Yeovil Fencing

Website for Yeovil Fencing, a fencing business in Yeovil, Somerset: https://yeovilfencing.com

Plain HTML and CSS in `site/`, with no build step. Cloudflare Pages serves `site/` from the `main` branch, so every push goes live.

```
python -m http.server 8081 --directory site   # preview at http://localhost:8081
```

Project rules and hosting details: [CLAUDE.md](CLAUDE.md).
