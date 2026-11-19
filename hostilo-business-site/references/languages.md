# Languages

## Folder structure

```
/index.html              default language home (e.g. Polish)
/o-nas.html              default language subpage   → served at /o-nas
/aktualnosci.html        default language news list
/kontakt.html
/en/index.html           English home               → /en/
/en/about.html           → /en/about
/en/news.html
/en/contact.html
/de/index.html …
/assets/site.css         shared by all languages
/assets/img/…
/404.html                custom "page not found" (optional, one file for all languages, link to every language home)
```

- One default language in the root. Do not put the default language in a folder too (no `/pl/` duplicate).
- Every language has the **same set of pages**. If a page exists only in one language, say so in `SITE.md`.
- Slugs: lowercase `a-z`, `0-9`, `-`, no diacritics (`o-nas`, not `o-nás`). Translated slugs are fine (`/en/about`, `/de/ueber-uns`); keep the mapping table in `SITE.md`.
- Proper names are not translated in slugs or titles (trade fair "Autostrada Polska" stays "Autostrada Polska").
- Hostilo redirects `/en` to `/en/` when `en/index.html` exists. Do not create redirect pages yourself.
- Do not auto-redirect visitors by browser language. Search engines and shared links must land on the page they asked for.

## In every page `<head>`

```html
<html lang="en">
…
<link rel="canonical" href="https://example.com/en/about">
<link rel="alternate" hreflang="pl" href="https://example.com/o-nas">
<link rel="alternate" hreflang="en" href="https://example.com/en/about">
<link rel="alternate" hreflang="de" href="https://example.com/de/ueber-uns">
<link rel="alternate" hreflang="x-default" href="https://example.com/o-nas">
```

- Absolute URLs with the final domain (custom domain once connected; until then `https://<name>.hostilo.page`). Note the domain in `SITE.md`; after connecting a custom domain, update canonical/hreflang on all pages in one publish.
- Every page lists all its language versions, including itself. `x-default` points to the default language.
- Hostilo builds `sitemap.xml` automatically from all HTML files (do not upload your own unless you must; your own file replaces the automatic one).

## Language switcher

- In the header of every page, links to **the same page** in the other languages (not to the other home page).
- Show language codes or names (`PL · EN · DE`), mark the current one with `aria-current="true"`, give each link `hreflang` and `lang` attributes.
- No flags as the only label (flags are countries, not languages).

```html
<nav class="lang" aria-label="Language">
  <a href="/o-nas" hreflang="pl" lang="pl">PL</a>
  <a href="/en/about" hreflang="en" lang="en" aria-current="true">EN</a>
  <a href="/de/ueber-uns" hreflang="de" lang="de">DE</a>
</nav>
```

## Content per language

- Translate everything visible: menu, footer, buttons, form labels and error texts, alt texts, `<title>`, meta description, Open Graph texts, 404 page links.
- Dates and numbers in the local format (pl `19.11.2026`, en `19 November 2026` or `2026-11-19`, de `19.11.2026`), set `date_format` per language in `_memory/posts.json`.
- Machine translation is fine for drafts; record in `SITE.md` who checks each language (e.g. "DE: checked by the export department").
- Legal pages (privacy policy, imprint) may stay in one language only if the owner decides so; link them from every language.
