# Estavle website

The Estavle home page ("Opción O": the eight-scene illustrated story of a family's first home), in Spanish and English.
A plain static site: no build step, no server code, no keys.

- Moved here on Nov 23 2026 from the Claude page where it was designed, so the Asset Building team (Max, through the Studio) can work on it.
- Nothing here deploys by itself. Going live, and pointing estavle.com at it, is Alvaro's call.

## What's in here

```
index.html            the whole page: styles, ES/EN copy and script in one file
404.html              "page not found" (served by most static hosts automatically)
favicon.ico           browser tab icon (fallback for old browsers)
robots.txt            lets search engines in, points to the sitemap
sitemap.xml           the one URL of the site
assets/
  brand/              logo mark (SVG favicon) and apple-touch-icon
  img/                the eight scene illustrations (il-1 … il-8) and og-image.jpg (link previews)
  img/sprites/        cut-paper layers (leaves, flower, hummingbird, keys, papers)
  video/              short loops for scenes 1, 3 and 8 (the illustration shows until they play)
```

All paths inside `index.html` are relative (`assets/...`), so the page works from any folder.
`404.html` uses root paths (`/assets/...`) because hosts show it at any missing URL; it expects the site to live at the domain root.
File names are lowercase; keep them that way (most hosts are case-sensitive).

## Run it locally

Any static file server works. From this folder:

```sh
npx serve .            # then open the URL it prints
# or
python3 -m http.server 8000   # then open http://localhost:8000
```

Opening `index.html` straight from disk also works, but a server is closer to what the live site does.

Handy while editing:

- `?skip` skips the typed intro: `http://localhost:8000/?skip`
- `#en` opens the English version: `http://localhost:8000/#en`
- The chosen language is remembered in the browser (`localStorage`, key `estavle-lang`).

## Deploy as a static site

Publish this folder as-is: no build command, output/publish directory = the repo root (`.`).

| Host | Settings |
| --- | --- |
| Netlify | Build command: *(empty)* · Publish directory: `.` |
| Vercel | Framework preset: *Other* · Build command: *(empty)* · Output directory: `.` |
| Cloudflare Pages | Framework preset: *None* · Build command: *(empty)* · Build output directory: `/` |
| GitHub Pages | Source: *Deploy from a branch* · `main` / `(root)` · use a custom domain (see below) |

All four serve `404.html` for missing pages automatically.

Before going live on a real domain:

1. The canonical URL, social preview tags (`og:url`, `og:image`, `twitter:image`), `robots.txt` and `sitemap.xml` all say **`https://estavle.com/`**. If the site lives somewhere else (for example `www.estavle.com`), change that URL in those four places.
2. GitHub Pages without a custom domain serves the site from `/<repo-name>/`; the main page still works there, but `404.html` (root paths) would lose its icon. Use a custom domain or one of the other hosts.

## Editing notes

- Copy lives in the `COPY` object near the top of the `<script>` in `index.html` (`es` and `en`). The source copy doc is `marketing/STORY-v2.md` (not in this repo).
- Yellow `[NEEDS: …]` tags in the copy are deliberate placeholders for facts and legal text still to come.
- The form is a preview: it does not send anything yet.
- Pictures for each scene are loaded only when that scene (or its neighbour) is shown; see `data-src` in the script.
