# Yeovil Fencing: project guide

One-page website for Yeovil Fencing, a fencing business in Yeovil, Somerset. Live at **https://yeovilfencing.com** (bare domain is the main address; www redirects to it).

The site is plain static HTML and CSS, made before this repo was reorganised. There is no build step: edit the files in `site/` directly.

## Folder layout
- `site/`: everything the browser gets. `index.html`, `style.css`, `img/`, `privacy/`, `thank-you/`, plus:
  - `_headers`: Cloudflare security headers
  - `404.html`: the not-found page (noindex). Without it, Cloudflare Pages would serve the home page for every unknown URL.
  - `sitemap.xml` and `robots.txt`: written by `python -I "D:\storage\Claude\Move Domains\tools\make_sitemap.py" site https://yeovilfencing.com`. Re-run it after adding a page.
- `.claude/launch.json`: "site" preview. Or run `python -m http.server 8081 --directory site`.

## Working style
- Plain English, short sentences, UK spelling.
- Don't change the business's wording, prices or contact details without asking Rich.
- Never put Rich's personal email address on the site.
- Keep it simple: static HTML, no frameworks.

## Hosting
- Code: GitHub **NoMhawk/yeovilfencing** (public). Branch `main`.
- Cloudflare Pages project **yeovilfencing**: Git-connected to that repo, no build command, output folder `site`. Every push to `main` goes live in about a minute. It also serves https://yeovilfencing.pages.dev.
- Custom domains on the Pages project: `yeovilfencing.com` and `www.yeovilfencing.com`. Cloudflare made the proxied DNS records for them (CNAMEs to `yeovilfencing.pages.dev`).
- Redirect rule "www to bare domain": `https://www.yeovilfencing.com/*` → `https://yeovilfencing.com/${1}`. It is a 301 and keeps the query string.
- Always Use HTTPS: on.
- Moved from GitHub Pages on 26 Nov 2026, after the domain's DNS moved from Fasthosts to Cloudflare the same day (records in `D:\storage\Claude\Move Domains\yeovilfencing.com\`). The old GitHub Pages DNS was:
  - apex A records 185.199.108.153, .109, .110, .111
  - `www` CNAME `nomhawk.github.io`
  - all DNS only
  - To undo, put those back as DNS only (in place of the two Pages CNAMEs), restore the `CNAME` file and the root layout, and turn GitHub Pages back on.
- GitHub Pages was turned off on 26 Nov 2026, and the old root copies and `CNAME` were removed.
- The old Fasthosts zone (not live, kept as a fallback) was also changed on 26 Nov 2026. Its apex A records now point to 104.21.63.137 and 172.67.145.194 (the other two removed), and its `www` CNAME to `yeovilfencing.pages.dev`. This is because some resolvers kept asking Fasthosts for up to 48 hours after the nameserver move. Its email records are unchanged (still livemail).

## Analytics
- Cloudflare Web Analytics, automatic setup. There's no script in the code and no cookies. The beacon is added by Cloudflare at the edge.
- Google Search Console: domain property `yeovilfencing.com` (already set up in 2025), verified by a `google-site-verification` TXT record in Cloudflare DNS. **Do not delete it.** Sitemap `https://yeovilfencing.com/sitemap.xml` submitted on 26 Nov 2026. The old `https://www.yeovilfencing.com/sitemap.xml` (from April 2025) is still listed there too.

## Email (not part of this site)
Email is **Cloudflare Email Routing** for receiving, and **SMTP2GO** for sending from Gmail. The records:
- MX `route1`, `route2`, `route3.mx.cloudflare.net`, DKIM TXT `cf2024-1._domainkey`
- SPF `v=spf1 include:_spf.mx.cloudflare.net include:spf.smtp2go.com ~all`
- `_dmarc` (Cloudflare DMARC Management)
- SMTP2GO CNAMEs `em1267897`, `s1267897._domainkey` and `link`
- Left over from Fasthosts and harmless: `livemail1-4._domainkey` CNAMEs and the `mail`, `mailserver`, `smtp`, `webmail` and `mcp` A records

All of these are DNS only. Don't change them when working on the website.

## Site notes
- Pages: `/` (one-page site), `/privacy/` (privacy notice), `/thank-you/` (after the form, noindex), `404.html`.
- **Contact form:** posts straight to Web3Forms (no script).
  - Account `hello@yeovilfencing.com`. Form "Yeovil Fencing contact form", access key in `index.html` (public, not a secret).
  - After sending, visitors go to `/thank-you/`.
  - A hidden `botcheck` box catches spam bots.
  - The privacy notice names Web3Forms and Cloudflare Web Analytics. Change it if either changes.
- No home address, phone number or WhatsApp on the site, at Rich's request (26 Nov 2026). Contact is the form plus hello@yeovilfencing.com.
- The home page has a canonical link (`https://yeovilfencing.com/`), as do the privacy and thank-you pages.

- `site/app.js` (same as yeovilhandyman.com) runs the mobile menu (open, close on link tap or Escape) and darkens the header after scrolling. On pages without the hero (privacy, thank-you) the header is always solid.
- Search results and link previews (26 Nov 2026, Rich chose a branded card; the old yourwebsite.com placeholders were removed the same day):
  - `site/favicon.svg` is a simple "YF" in white on dark grey, matching Yeovil Handyman's "YH". Rich to update: swap in a real logo when there is one, then remake the icons.
  - `favicon.ico`, `apple-touch-icon.png`, `icon-192/512.png`, `logo-512.png` and `og-image.png` (dark card: YF, "Yeovil Fencing", "Garden and security fencing in Yeovil") are made by `python "D:\storage\Claude\Move Domains\tools\make_icons.py" --svg site/favicon.svg --out site --bg "#1f262e" --accent "#b8bec6" --card-title "Yeovil Fencing" --card-sub "Garden and security fencing in Yeovil"`.
  - The tags in each page's `<!-- search-appearance -->` block (icons, manifest, og:/twitter:, and WebSite + business schema on the home page) are written by `search_tags.py` in the same folder. Rerun it after adding a page or changing a title or description (in Git Bash, prefix `MSYS_NO_PATHCONV=1`).
  - Check it live: `node "D:\storage\Claude\Check Sites\scripts\search-appearance.mjs" yeovilfencing.com`.

- The hero background is `img/gravel.jpg` under a dark overlay (27 Nov 2026). The template's `hero-bg.png` and `img-1.png` (service cards) never existed; the service cards use the plain dark gradient, as on yeovilhandyman.com.

## Known issues (not fixed yet)
- The four service icons load from img.icons8.com (a third-party load).
- The project pictures in `site/img/`: check whether they are real job photos, stock or AI-made before claiming they show the business's work.
