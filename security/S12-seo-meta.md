Status: TODO

# S12 Missing share and search tags (Low)

## Problem
A shared notes-site link shows no preview card, and the store gives search engines no sitemap or structured data.

## Evidence (11-11-2026)
- Notes sites: `app/index.html` has only `<title>` and a description. No `og:` or `twitter:` tags, no favicon
  link, no `robots.txt` in `app/`.
- Store: `src/layouts/Base.astro:23-33` has title, description, canonical, OG and Twitter tags (good). The built
  `dist/` has no `sitemap.xml` and no `robots.txt`; no file under `src/` contains JSON-LD.

## Fix
Notes sites (they sit behind sign-in, so the aim is a good share card, not ranking):
1. Add to `app/index.html`: `og:title`, `og:description`, `og:image` (reuse the store's
   `https://notix-notes.vercel.app/og/<code>.png`), `og:url`, `twitter:card`, a favicon link, `theme-color`.
2. Add `app/robots.txt` allowing `/` and disallowing `/api/`, `/data/`, `/img/`, `/admin.html`.
Store (safe: no payment code):
3. Add `@astrojs/sitemap` to `astro.config.mjs` (set `site`), or write `src/pages/sitemap.xml.js` by hand from
   `src/data/subjects.mjs`. Add `public/robots.txt` pointing to it and disallowing `/api/` and `/admin`.
4. JSON-LD in `src/pages/[code].astro`: `Product` with `name`, `description`, `offers` (price from the same
   source the page uses, currency INR), and `aggregateRating` only when approved reviews exist. `Organization`
   and `WebSite` in `Base.astro`. `FAQPage` from `src/components/Faq.astro` data.

## Files
Sites: `app/index.html`, `app/robots.txt`. Store: `astro.config.mjs`, `src/layouts/Base.astro`,
`src/pages/[code].astro`, `public/robots.txt`, `package.json` (if the sitemap package is added).

## Prove it
`npm run build` then check `dist/sitemap*.xml` lists `/`, each subject, `/requests`, `/updates`, legal pages.
Paste a built subject page into Google's Rich Results Test (owner or browser). For sites: view source shows the tags.

## Rollback
`git revert`.

## Done when
Build output contains the sitemap and JSON-LD; the four sites have OG tags; committed.
