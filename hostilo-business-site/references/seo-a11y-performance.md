# SEO, accessibility, performance, analytics

## `<head>` of every page

```html
<!doctype html>
<html lang="pl">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>O nas | Stalbud Przepusty</title>                       <!-- unique, up to ~60 characters -->
  <meta name="description" content="…">                          <!-- unique, up to ~155 characters -->
  <link rel="canonical" href="https://example.com/o-nas">
  <!-- hreflang links: languages.md -->
  <meta property="og:type" content="website">
  <meta property="og:title" content="O nas | Stalbud Przepusty">
  <meta property="og:description" content="…">
  <meta property="og:url" content="https://example.com/o-nas">
  <meta property="og:image" content="https://example.com/assets/img/og.jpg">   <!-- 1200×630; Hostilo makes one automatically if missing -->
  <link rel="icon" href="/assets/img/favicon.svg" type="image/svg+xml">
  <link rel="stylesheet" href="/assets/site.css">
</head>
```

## Structured data (JSON-LD)

- Home page: `Organization` (or `LocalBusiness`) with name, logo, address, phone, `sameAs` (social profiles).
- Post pages: `NewsArticle` with headline, datePublished, image, author/publisher (put it in the post template with placeholders).
- Contact page: the same organization data visible on the page.
- Only facts that are on the page. No invented ratings or reviews.

## Content

One `<h1>` per page, logical `<h2>`/`<h3>`, descriptive link texts ("Read the full post", not "click here"), real company data (no placeholders left in production).

## Images

- WebP, max 2000 px wide, usually under 300 KB; hero images under 500 KB.
- Always `width` and `height` attributes (no layout jumps) and `alt` text in the page language (empty `alt=""` for decorative images).
- `loading="lazy"` for images below the first screen; not for the main hero image.
- Upload from the user via the Hostilo upload link (converted to WebP automatically).

## Fonts and scripts

- System fonts, or self-hosted `woff2` in `/assets/fonts/` with `font-display: swap`. Avoid loading many families/weights.
- Little JavaScript: menu toggle, form sending. No heavy frameworks for a company site.

## Accessibility (WCAG 2.2 AA)

Text contrast at least 4.5:1, visible focus styles, "skip to content" link, menu usable by keyboard, labels for all form fields, `aria-current="page"` in the menu, language switcher with `lang`/`hreflang`, no text in images, works at 375 px without horizontal scroll and at 200 % zoom.

## Analytics and cookies

- Hostilo has built-in statistics without cookies (no consent banner needed). Prefer them.
- If the owner wants Google Analytics 4 or Tag Manager: add a consent banner (EU) with Consent Mode v2, default `denied`, scripts loaded only after consent; same snippet on every page (in the header partial); record the IDs in `SITE.md`.
- Embedded YouTube/maps: use privacy-friendly embeds (`youtube-nocookie.com`) or a click-to-load placeholder.

## For AI assistants and search

- Hostilo serves `robots.txt`, `sitemap.xml` and IndexNow automatically (do not upload your own unless required).
- `llms.txt` (summary of the site for AI assistants) can be switched on by the owner in the Hostilo dashboard (site Settings → "For AI").
