# Laxmi Veneer & Timber LLP — website

Static single-page site (no build step, no framework). Everything is plain HTML/CSS/JS,
so it deploys on Vercel as-is.

## Structure

```
index.html            the site (content, SEO meta, JSON-LD)
404.html              custom not-found page (Vercel serves it automatically)
vercel.json           security headers + caching rules
robots.txt            crawler rules
site.webmanifest      PWA / "add to home screen" metadata
favicon.ico           browser tab icon
og-image.jpg          link-preview image for WhatsApp / social (1200x630)
assets/
  css/styles.css      all styles
  js/boot.js          tiny head script (sets the .js class before first paint)
  js/main.js          all behaviour (nav, lightbox, guides, slideshow, enquiry)
  icons/              favicon PNGs + app icons
  img/                logo, texture backgrounds, product-card previews
images/               product photography, loaded lazily by the page
```

## Deploy

1. Go to https://vercel.com/new and drag this whole folder in, **or** run
   `npx vercel` from this folder and accept the defaults (framework preset: Other,
   no build command, output directory: root).
2. Done. No environment variables are needed.

## After you connect a domain (optional)

- Add `<link rel="canonical" href="https://your-domain/">` in `index.html` `<head>`.
- Make the share image absolute so every platform can resolve it:
  change `content="og-image.jpg"` to `content="https://your-domain/og-image.jpg"`
  (`og:image` + `twitter:image`).
- Add a `sitemap.xml` and reference it from `robots.txt`.

## Security

`vercel.json` applies to every response:

- **Content-Security-Policy** — scripts only from this site (no inline JS), images from
  this site + Unsplash, frames only Google Maps, fonts from Google Fonts. It also blocks
  `object`/`embed`, base-tag hijacking and framing of the site.
- **Strict-Transport-Security** (2 years), **X-Content-Type-Options: nosniff**,
  **X-Frame-Options: DENY**, **Referrer-Policy: strict-origin-when-cross-origin**,
  **Permissions-Policy** (camera, mic, geolocation, payment, usb off).

If you ever add an external script/iframe/font, add its host to the matching CSP
directive in `vercel.json` or it will be blocked.

## Caching

- `/assets/*` and `/images/*` — cached 1 week with `stale-while-revalidate`
  (returning visitors get instant loads, updates arrive in the background).
- HTML — Vercel's default: always revalidated, so content edits appear immediately.

## Editing

- Styles → `assets/css/styles.css`
- Behaviour → `assets/js/main.js` (WhatsApp number: the `WHATSAPP` constant)
- Product photos → `images/` (referenced from the `IMAGES` arrays in `main.js`;
  keep photos ~300 KB or less and 1600 px wide max)
- Keep `index.html`, `assets/` and `images/` in the same relative layout when
  renaming anything — all references are relative.
