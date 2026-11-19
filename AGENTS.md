# AGENTS.md: Hostilo Business site playbook

> Single-file version of the Claude skill `hostilo-business-site` for Codex, Cursor, Windsurf, ChatGPT Projects and other AI coding tools. Put it in the root of the website project as `AGENTS.md`, or upload it to a ChatGPT Project. The complete starter site is in the skill package (`starter/`). "references/x.md" below means the section of this file with that name.

You are building or maintaining a static company website that is hosted on Hostilo and will be run through AI assistants (ChatGPT, Claude, Gemini, Codex, Cursor…) for years, often by different people. Everything you do must keep the site **consistent, multilingual, fast and understandable for the next person or AI**.

Answer the user in their language. Keep code, file names and these rules in English.

## The 12 rules (always)

1. **Static files only.** HTML, CSS, JS, images (WebP preferred), fonts (woff2), json, txt, xml, pdf. No PHP, no server code, no database. If you use a framework, publish its built output.
2. **One folder per language.** Default language in the site root (`/index.html`, `/o-nas.html`), every other language in its own folder (`/en/index.html`, `/en/about.html`, `/de/…`). See `references/languages.md`.
3. **Clean, root-relative links.** Link to `/en/about`, not `about.html` or `../about.html`. Assets: `/assets/site.css`, `/assets/img/logo.webp`. Hostilo serves `about.html` at `/about` and redirects `/en` to `/en/`.
4. **Shared assets in `/assets/`.** One stylesheet for the whole site (`/assets/site.css`), one script if needed (`/assets/site.js`). A new look should usually mean a new stylesheet, not rewritten pages.
5. **Identical header and footer on every page** of a language, marked with `<!-- header -->…<!-- /header -->` and `<!-- footer -->…<!-- /footer -->`. When the menu changes, change it on every page in one publish.
6. **Every page:** `<html lang>`, unique `<title>` and meta description, one `<h1>`, canonical, `hreflang` links to all language versions, Open Graph tags, responsive at 375 px without horizontal scroll. See `references/seo-a11y-performance.md`.
7. **Contact forms use the Hostilo form endpoint**, never `mailto:`. See `references/forms.md`.
8. **News posts are content, not pages** (Business): publish `_posts/<lang>/<slug>.html`, never hand-edit post pages or news lists. See `references/posts-and-memory.md`.
9. **The site memory is the source of truth.** Read `SITE.md` and `DESIGN.md` (shown at the top of `get_site`) before any change; update them after any bigger change. See `references/posts-and-memory.md`.
10. **Publish only what you changed** with `publish_files` (without `index.html` unless the home page changed) and always pass `note`: one sentence, what and why. See `references/publishing.md`.
11. **Big changes are reversible.** Hostilo snapshots the whole site before publishing 5+ files or templates; a redesign follows `references/redesign.md`.
12. **Before saying "done"**, run `references/checklist.md` on the live URL.

## Workflow by task

| Task | Do this |
|---|---|
| New site from scratch | Copy the structure of `starter/`. Build the default language first, then translate page by page. Publish everything in one `publish_files` call (or `import_zip`). Then write `_memory/SITE.md` and `_memory/DESIGN.md`. |
| Migrate an existing site (WordPress, old HTML) | Keep every old URL working: same slugs where possible, otherwise list old → new addresses in `SITE.md`. Move news into `_posts/`. Keep texts; do not "improve" content without asking. |
| Add a news post | `get_site` (read memory), then one `_posts/<lang>/<slug>.html` per language in one `publish_files` call, with `note`. Photo from the user: give them the upload link shown in `get_site`. |
| Change one page | `get_site` with `path` → edit → `publish_files` with that single file and `note`. |
| Add a page in all languages | Create the page in every language, add it to the menu on every page, publish all changed files in one call, update the page map in `SITE.md`. |
| Translate / add a language | New folder `/<lang>/`, all pages, menu with language switcher on every page in every language, `hreflang` everywhere, new entry in `_memory/posts.json`, translated post sources in `_posts/<lang>/`. |
| Redesign | `references/redesign.md`. |
| Something broke | `list_versions` → `restore_version` (ids starting with `snap-` restore the whole site). Then explain what happened in `note`. |

## Hostilo tools (connector / REST API)

`list_sites`, `get_site` (home page + file list + site memory + form details; with `path` one file, also `_memory/…` and `_posts/…`), `publish_files`, `update_site` (home page only), `import_zip`, `create_site`, `list_templates`, `list_versions`, `restore_version`, `get_site_stats`, `get_ai_traffic`. Details: `references/publishing.md`.

## Limits worth knowing

300 files and 20 MB per publish, 10 MB per file, HTML page up to 5 MB, storage per plan (Free 50 MB, PRO 1 GB, Business 5 GB). Forms: Free 50 messages a month, PRO and Business unlimited, 10 per hour per visitor IP.

## References

- `references/languages.md`: folders, slugs, hreflang, language switcher, dates
- `references/forms.md`: Hostilo form endpoint, fields, thank-you pages, fetch, consent, CRM forms
- `references/posts-and-memory.md`: SITE.md, DESIGN.md, posts.json, templates, markers, post files
- `references/publishing.md`: tools, notes, versions, snapshots, uploads
- `references/redesign.md`: changing the look of the whole site safely
- `references/seo-a11y-performance.md`: head tags, structured data, images, fonts, accessibility, analytics and cookies
- `references/checklist.md`: before handover and after every bigger change
- `starter/`: a complete minimal site (PL root + EN), memory, templates and one post, ready to copy

---

<!-- references/languages.md -->
## Languages

### Folder structure

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

### In every page `<head>`

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

### Language switcher

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

### Content per language

- Translate everything visible: menu, footer, buttons, form labels and error texts, alt texts, `<title>`, meta description, Open Graph texts, 404 page links.
- Dates and numbers in the local format (pl `19.11.2026`, en `19 November 2026` or `2026-11-19`, de `19.11.2026`), set `date_format` per language in `_memory/posts.json`.
- Machine translation is fine for drafts; record in `SITE.md` who checks each language (e.g. "DE: checked by the export department").
- Legal pages (privacy policy, imprint) may stay in one language only if the owner decides so; link them from every language.

---

<!-- references/forms.md -->
## Contact forms

Hostilo sites have a built-in form endpoint: no server code, messages go to the site owner's e-mail and to the Hostilo dashboard (CSV export and webhook on PRO and Business).

### Minimal form (works without JavaScript)

```html
<form action="https://hostilo.app/api_form.php" method="POST" class="contact-form">
  <input type="hidden" name="site_token" value="SITE_TOKEN">
  <input type="hidden" name="_next" value="https://example.com/dziekujemy">
  <!-- anti-spam trap: keep hidden, never fill -->
  <input type="text" name="_honey" tabindex="-1" autocomplete="off" style="display:none">

  <label for="f-name">Imię i nazwisko</label>
  <input id="f-name" name="name" autocomplete="name">

  <label for="f-email">E-mail *</label>
  <input id="f-email" name="email" type="email" required autocomplete="email">

  <label for="f-company">Firma</label>
  <input id="f-company" name="company" autocomplete="organization">

  <label for="f-msg">Wiadomość *</label>
  <textarea id="f-msg" name="message" required maxlength="5000"></textarea>

  <label><input type="checkbox" name="consent" value="yes" required>
    Zgadzam się na kontakt w sprawie zapytania. <a href="/polityka-prywatnosci">Polityka prywatności</a></label>

  <button type="submit">Wyślij</button>
</form>
```

### Fields

| Field | Rule |
|---|---|
| `site_token` | required, hidden. The site's own token: shown by `get_site` ("Contact form" section) and in the Hostilo dashboard (site Settings → form). The same token for every language. It is public by design. |
| `email` | required, valid e-mail. Replies go to this address. |
| `message` | required, up to 5000 characters. |
| `name` | optional. |
| other fields | up to 20 extra fields, names without a leading `_`, up to 50 characters, values up to 1000 characters (e.g. `company`, `phone`, `product`, `consent`). |
| `_honey` | anti-spam trap, must stay empty and hidden. |
| `_next` | optional thank-you page: full `https://` URL **on the same site** (subdomain or connected custom domain). One per language (`/dziekujemy`, `/en/thank-you`). Without it Hostilo shows its own thank-you page in the visitor's browser language. Give thank-you pages `<meta name="robots" content="noindex">`. |

### Rules

- **Never** `mailto:` forms, never third-party form services when the Hostilo form is enough.
- The endpoint accepts posts only from the site's own addresses (Origin check). Test on the live site, not from a local file.
- Label every field (`<label for>`), mark required fields, use `autocomplete`, show errors in the page language.
- Consent checkbox with a link to the privacy policy for every form that collects personal data (EU). Do not pre-tick it. Marketing consent is a separate, optional checkbox.
- Limits: Free 50 messages a month per account; PRO and Business unlimited; 10 messages per hour per visitor IP.
- Each language version has its own form page with translated labels and its own `_next`.

### With JavaScript (no page reload)

```js
form.addEventListener('submit', async (e) => {
  e.preventDefault();
  const r = await fetch(form.action, { method: 'POST', body: new FormData(form), headers: { Accept: 'application/json' } });
  const j = await r.json(); // { status: 'success' | 'error', message }
  // show j.status === 'success' ? thank-you text : error text (in the page language, not j.message)
});
```

### CRM and other forms

Embedded forms from a CRM (HubSpot, Pipedrive, Brevo…) work: external scripts are allowed on Hostilo sites. Record the form id and where it is used in `SITE.md`. If the customer wants messages in their CRM but a simple form on the site, use the Hostilo form with the webhook (PRO/Business) to Make, n8n or Zapier.

---

<!-- references/posts-and-memory.md -->
## Site memory and news posts (Hostilo Business)

These features are active only on sites whose owner has the Hostilo Business plan. `get_site` then starts with a block "HOSTILO BUSINESS SITE: READ THIS FIRST". Without the plan, `_memory/` and `_posts/` are rejected.

Memory files and post sources are stored outside the public site (never served to visitors), survive `clear_assets` and redesigns, and are part of every full-site snapshot. Read any of them with `get_site` and `path` (e.g. `_memory/templates/post.html`). Write them with `publish_files` (paths `_memory/…`, `_posts/…`).

### `_memory/SITE.md` (required)

The site's instruction manual for the next person or AI. Keep it short and factual. Template:

```markdown
## <Company name>: website memory
### Company
What the company does, for whom, key products/services, tone (formal/informal, "we"), words to use and to avoid.
### Domain and languages
Live address, default language, other languages, who checks each language.
### Page map
| page | pl | en | de |
|---|---|---|---|
| Home | / | /en/ | /de/ |
| About | /o-nas | /en/about | /de/ueber-uns |
### Structure rules
Shared header/footer, stylesheet /assets/site.css, menu order, what must not be changed without the owner.
### Integrations
Form: Hostilo form (token from get_site), thank-you pages. Analytics: GA4 G-XXXX with consent banner (or Hostilo statistics only). CRM: … Fonts: …
### News
How often, typical topics, who approves, photo rules.
### History of big decisions
2026-11-19: site migrated from WordPress, old URLs kept (list below if changed).
```

### `_memory/DESIGN.md` (required)

Colours (hex), fonts, sizes, spacing, components (header, buttons, cards, news item, form), image rules (format WebP, max width 2000 px, aspect ratios), dark mode yes/no. Update it after every redesign.

### `_memory/posts.json`

```json
{
  "languages": {
    "pl": { "post_path": "aktualnosci/{slug}.html", "lists": ["aktualnosci.html", "index.html"], "feed": "aktualnosci/feed.xml", "feed_title": "Aktualności", "date_format": "DD.MM.YYYY" },
    "en": { "post_path": "en/news/{slug}.html", "lists": ["en/news.html", "en/index.html"], "feed": "en/news/feed.xml", "feed_title": "News", "date_format": "YYYY-MM-DD" }
  }
}
```

- `post_path` must contain `{slug}` and end with `.html`. During a migration you may keep old URLs (e.g. `"aktualnosc-{slug}.html"`).
- `lists`: pages that contain a list of posts (news page, home page teaser).
- `feed`: RSS file (optional).

### Templates

| File | Used for |
|---|---|
| `_memory/templates/post.html` or `post.<lang>.html` | the full page of one post (a complete HTML document with the site header and footer) |
| `_memory/templates/post-item.html` or `post-item.<lang>.html` | one item in a list |

Placeholders: `{{title}}`, `{{date}}` (formatted by `date_format`), `{{date_iso}}`, `{{summary}}`, `{{content}}` (HTML, inserted as is), `{{image}}`, `{{image_alt}}`, `{{url}}` (clean root-relative link), `{{lang}}`, `{{slug}}`. Block shown only when a value exists: `{{#image}}<img src="{{image}}" alt="{{image_alt}}" width="1600" height="900">{{/image}}`. All values except `content` are HTML-escaped.

Template rules:
- Copy the real header/footer of the language into `post.<lang>.html` (so posts look like the rest of the site), with `hreflang` for the same post in other languages if you keep the same slug.
- **Always** include the image block. Hostilo warns when a post has an image but the template has no `{{image}}`.
- Root-relative links only (`/assets/site.css`).

### List markers

In every page listed in `lists`, put a marker pair where the list goes:

```html
<!-- hostilo:posts -->
<!-- /hostilo:posts -->

<!-- home page: only the 3 newest -->
<!-- hostilo:posts limit=3 -->
<!-- /hostilo:posts -->
```

Everything between the markers is replaced on every rebuild. Never edit it by hand.

### A post file: `_posts/<lang>/<slug>.html`

```html
<!--
title: Our booth at Autostrada Polska 2026
date: 2026-11-19
summary: One sentence for the list and the RSS feed.
image: /assets/uploads/2026-11/booth.webp
image_alt: Booth B-24 at Autostrada Polska 2026
-->
<p>Text of the post in HTML: paragraphs, lists, links, more images.</p>
```

- `title` and `date` (YYYY-MM-DD) are required. Slug: lowercase `a-z 0-9 -`, max 100 characters; same slug in every language when possible.
- One file per language, all languages in one `publish_files` call, with `note`.
- **Delete** a post: publish the same path with empty content.
- **Edit** a post: read it with `get_site` path `_posts/<lang>/<slug>.html`, publish the whole changed file.
- Photos from the user: give them the upload link from `get_site` (valid 2 hours); uploaded files appear in the file list under `assets/uploads/<year-month>/` as WebP.

Hostilo then rebuilds the post pages, every list and the RSS feed for that language. Any change to `posts.json` or a template rebuilds all posts in all languages.

---

<!-- references/publishing.md -->
## Publishing with the Hostilo connector or REST API

Connect Hostilo in Claude (Connectors directory, "Hostilo"), ChatGPT (Apps, "Hostilo"), or any MCP client at `https://hostilo.app/mcp` (OAuth). REST API with a key `hk_…`: `https://hostilo.app/api/v1/<action>`, OpenAPI at `https://hostilo.app/openapi.json`.

### Tools

| Tool | Use |
|---|---|
| `list_sites` | find the site (id or name) |
| `get_site` | without `path`: home page HTML, list of all files, form details and (Business) the site memory with rules and recent changes. With `path`: one text file (`en/about.html`, `assets/site.css`, `_memory/DESIGN.md`, `_posts/pl/x.html`) |
| `publish_files` | publish files to an existing site. Only a file named `index.html` changes the home page; without it only the files you send are added or replaced. HTML pages keep their folders and get clean URLs; other files go to `assets/` with folders kept and links are fixed. Binary files as base64. `dry_run` previews. |
| `update_site` | replace only the home page HTML |
| `import_zip` | a whole project from a ZIP (base64 or public URL) |
| `create_site` | new site (html, files or a Hostilo template) |
| `list_versions` / `restore_version` | home page versions; on Business also full-site snapshots `snap-…` (restore the whole site) |
| `get_site_stats` / `get_ai_traffic` | visitors and AI bots |

### Rules

- **Read before you write.** `get_site` first (memory and file list), then `get_site` with `path` for each file you change. Never rebuild a page from memory or from a screenshot.
- **Publish only changed files**, all related files in one call (e.g. a new page in 3 languages + menu changes).
- **Always pass `note`**: one sentence, what and why ("Added Infrastruktura 2026 post in PL/EN/DE with booth photo"). It goes to the change log that the next AI reads.
- If a response says `TRUNCATED`, the file was cut: do not publish it back.
- Pass `expected_version` (from `get_site`) when several people work on the site; on `version_conflict` read again and merge.
- `clear_assets: true` deletes all files in `assets/` before publishing: only in a full redesign that re-sends every asset.
- Uploaded photos (`assets/uploads/…`) are never deleted by normal publishes.
- After publishing, open the live URL and run the checklist.

### Snapshots and undo (Business)

Hostilo takes a full-site snapshot (all pages, assets, memory) automatically before a publish of 5+ files, with `clear_assets`, or with template changes. `list_versions` shows them first; `restore_version` with `snap-…` restores everything and keeps the current state as a new snapshot, so restoring can be undone too.

---

<!-- references/redesign.md -->
## Redesign: a new look for the whole site

A redesign years later must not lose content, languages, URLs or posts. Follow these steps in order.

1. **Read.** `get_site` (memory, file list, recent changes). Read `SITE.md`, `DESIGN.md`, `posts.json`, the templates and every page (`get_site` with `path`). Write down the page map and all texts.
2. **Agree the scope** with the user: only the look (stylesheet, header/footer, components) or also structure and texts? Default: only the look.
3. **Prefer CSS.** If the HTML structure is clean, a new `/assets/site.css` plus small header/footer changes is the safest redesign. Keep class names used by templates and lists.
4. **Keep URLs.** Same slugs, same language folders. If a URL must change, keep a short page at the old address linking to the new one and list both in `SITE.md`.
5. **Update templates** (`_memory/templates/post*.html`, `post-item*.html`) to the new look, with the image block. Hostilo rebuilds all posts in all languages automatically.
6. **Publish in one call**: stylesheet, changed pages in every language, templates, updated `_memory/DESIGN.md` and (if structure changed) `_memory/SITE.md`, with `note` ("Redesign 2029: light theme, green accent, all content kept"). Hostilo takes a snapshot first; write its id in your answer to the user.
7. **Check** every language on the live site at 375 px and desktop (`checklist.md`), including a post page, a list page, the form and the 404 page.
8. **If the user does not like it**: `list_versions` → `restore_version` with the snapshot id from step 6. The new look stays available as another snapshot.

Never: rewrite texts "while you are at it", drop a language, remove posts, change the form token, or upload your own `sitemap.xml` during a redesign.

---

<!-- references/seo-a11y-performance.md -->
## SEO, accessibility, performance, analytics

### `<head>` of every page

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

### Structured data (JSON-LD)

- Home page: `Organization` (or `LocalBusiness`) with name, logo, address, phone, `sameAs` (social profiles).
- Post pages: `NewsArticle` with headline, datePublished, image, author/publisher (put it in the post template with placeholders).
- Contact page: the same organization data visible on the page.
- Only facts that are on the page. No invented ratings or reviews.

### Content

One `<h1>` per page, logical `<h2>`/`<h3>`, descriptive link texts ("Read the full post", not "click here"), real company data (no placeholders left in production).

### Images

- WebP, max 2000 px wide, usually under 300 KB; hero images under 500 KB.
- Always `width` and `height` attributes (no layout jumps) and `alt` text in the page language (empty `alt=""` for decorative images).
- `loading="lazy"` for images below the first screen; not for the main hero image.
- Upload from the user via the Hostilo upload link (converted to WebP automatically).

### Fonts and scripts

- System fonts, or self-hosted `woff2` in `/assets/fonts/` with `font-display: swap`. Avoid loading many families/weights.
- Little JavaScript: menu toggle, form sending. No heavy frameworks for a company site.

### Accessibility (WCAG 2.2 AA)

Text contrast at least 4.5:1, visible focus styles, "skip to content" link, menu usable by keyboard, labels for all form fields, `aria-current="page"` in the menu, language switcher with `lang`/`hreflang`, no text in images, works at 375 px without horizontal scroll and at 200 % zoom.

### Analytics and cookies

- Hostilo has built-in statistics without cookies (no consent banner needed). Prefer them.
- If the owner wants Google Analytics 4 or Tag Manager: add a consent banner (EU) with Consent Mode v2, default `denied`, scripts loaded only after consent; same snippet on every page (in the header partial); record the IDs in `SITE.md`.
- Embedded YouTube/maps: use privacy-friendly embeds (`youtube-nocookie.com`) or a click-to-load placeholder.

### For AI assistants and search

- Hostilo serves `robots.txt`, `sitemap.xml` and IndexNow automatically (do not upload your own unless required).
- `llms.txt` (summary of the site for AI assistants) can be switched on by the owner in the Hostilo dashboard (site Settings → "For AI").

---

<!-- references/checklist.md -->
## Checklist (live site, before handover and after every bigger change)

### Structure and links
- [ ] Every language has the same pages; menu and footer identical on all pages of a language.
- [ ] All internal links root-relative and clean (`/en/about`), no `.html`, no broken links (check the menu, footer, language switcher, buttons).
- [ ] Language switcher leads to the same page in the other language; current language marked.
- [ ] `/en` → `/en/` works; 404 page shows links to every language home.

### Head and SEO
- [ ] `<html lang>` correct on every page; unique title and description per page and language.
- [ ] Canonical and hreflang (all languages + x-default) with the final domain.
- [ ] Open Graph tags; JSON-LD Organization on the home page, NewsArticle in the post template.

### Forms
- [ ] Hostilo endpoint, correct `site_token`, `_honey` present and hidden, `email` and `message` required, labels, consent with privacy link.
- [ ] `_next` thank-you page per language exists, is on the same domain and has `noindex`.
- [ ] Test message sent from the live site in each language and received by the owner.

### News (Business)
- [ ] `get_site` shows SITE.md, DESIGN.md, posts.json and templates without warnings.
- [ ] Lists contain `<!-- hostilo:posts -->` markers; newest post first; home page teaser limited.
- [ ] A test post with an image shows the image on the post page and in the list; RSS opens.

### Look and quality
- [ ] 375 px (phone) and desktop: no horizontal scroll, readable text, tap targets ≥ 44 px.
- [ ] Contrast ≥ 4.5:1, visible focus, images with alt text and width/height, WebP.
- [ ] No placeholder texts, no "lorem ipsum", no invented facts or reviews.
- [ ] Analytics only with consent (if GA4/GTM is used).

### Memory and handover
- [ ] `SITE.md` and `DESIGN.md` describe the current state (page map, integrations, decisions).
- [ ] Last publish had a `note`; the user knows the snapshot id of the last big change.
- [ ] Owner knows how to add a post: "Add a news post on <site> in all languages: …".
