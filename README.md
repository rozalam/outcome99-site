# Outcome99 Site

Single-page horizontal-deck presentation site. Optimized for fast loads via external font files, optimized video, and aggressive caching on static assets.

## Structure

```
.
├── index.html        # ~40 KB — page markup, styles, scripts
├── hero.mp4          # ~516 KB — autoplay loop on slide 1 (no audio, h264, web-optimized)
├── fonts/            # ~230 KB total — Geist family, served separately so the browser can cache
│   ├── Geist-Light.woff2     (300)
│   ├── Geist-Regular.woff2   (400) — preloaded
│   ├── Geist-Italic.woff2    (400 italic)
│   ├── Geist-Medium.woff2    (500) — preloaded
│   ├── Geist-SemiBold.woff2  (600)
│   ├── Geist-Bold.woff2      (700)
│   └── Geist-Black.woff2     (900)
└── vercel.json       # Cache headers (1-year immutable on fonts/video, no-cache on HTML)
```

## Deploy

Push to GitHub, import the repo in Vercel, deploy. No build step — it's a static site.

## Updating

Edit `index.html` and push. Vercel auto-deploys on every push to `main`. Because `vercel.json` sends no-cache headers for HTML and 1-year immutable for assets, users always get the latest HTML but fonts/video stay cached across visits.

If you ever need to swap the hero video or a font file, rename the new file (e.g. `hero-v2.mp4`) and update the reference in `index.html` — the rename busts any stale cache.

## Optimization notes

- Hero video re-encoded from 1.7 MB to 516 KB by stripping the redundant audio track (video is muted) and using CRF 28 with web-optimized faststart.
- Fonts extracted from base64 (was 2.6 MB inline) to separate woff2 files (~33 KB each) with `font-display: swap` so text renders immediately in fallback while Geist loads.
- `<link rel="preload">` hints for the two most-used font weights and the hero video so the browser starts fetching them in parallel with the HTML parse.
- Total first-load transfer: ~625 KB (HTML + preloaded fonts + video), down from a single 2.7 MB file.
