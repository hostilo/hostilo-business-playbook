# Site memory and news posts (Hostilo Business)

These features are active only on sites whose owner has the Hostilo Business plan. `get_site` then starts with a block "HOSTILO BUSINESS SITE: READ THIS FIRST". Without the plan, `_memory/` and `_posts/` are rejected.

Memory files and post sources are stored outside the public site (never served to visitors), survive `clear_assets` and redesigns, and are part of every full-site snapshot. Read any of them with `get_site` and `path` (e.g. `_memory/templates/post.html`). Write them with `publish_files` (paths `_memory/…`, `_posts/…`).

## `_memory/SITE.md` (required)

The site's instruction manual for the next person or AI. Keep it short and factual. Template:

```markdown
# <Company name>: website memory
## Company
What the company does, for whom, key products/services, tone (formal/informal, "we"), words to use and to avoid.
## Domain and languages
Live address, default language, other languages, who checks each language.
## Page map
| page | pl | en | de |
|---|---|---|---|
| Home | / | /en/ | /de/ |
| About | /o-nas | /en/about | /de/ueber-uns |
## Structure rules
Shared header/footer, stylesheet /assets/site.css, menu order, what must not be changed without the owner.
## Integrations
Form: Hostilo form (token from get_site), thank-you pages. Analytics: GA4 G-XXXX with consent banner (or Hostilo statistics only). CRM: … Fonts: …
## News
How often, typical topics, who approves, photo rules.
## History of big decisions
2026-11-19: site migrated from WordPress, old URLs kept (list below if changed).
```

## `_memory/DESIGN.md` (required)

Colours (hex), fonts, sizes, spacing, components (header, buttons, cards, news item, form), image rules (format WebP, max width 2000 px, aspect ratios), dark mode yes/no. Update it after every redesign.

## `_memory/posts.json`

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

## Templates

| File | Used for |
|---|---|
| `_memory/templates/post.html` or `post.<lang>.html` | the full page of one post (a complete HTML document with the site header and footer) |
| `_memory/templates/post-item.html` or `post-item.<lang>.html` | one item in a list |

Placeholders: `{{title}}`, `{{date}}` (formatted by `date_format`), `{{date_iso}}`, `{{summary}}`, `{{content}}` (HTML, inserted as is), `{{image}}`, `{{image_alt}}`, `{{url}}` (clean root-relative link), `{{lang}}`, `{{slug}}`. Block shown only when a value exists: `{{#image}}<img src="{{image}}" alt="{{image_alt}}" width="1600" height="900">{{/image}}`. All values except `content` are HTML-escaped.

Template rules:
- Copy the real header/footer of the language into `post.<lang>.html` (so posts look like the rest of the site), with `hreflang` for the same post in other languages if you keep the same slug.
- **Always** include the image block. Hostilo warns when a post has an image but the template has no `{{image}}`.
- Root-relative links only (`/assets/site.css`).

## List markers

In every page listed in `lists`, put a marker pair where the list goes:

```html
<!-- hostilo:posts -->
<!-- /hostilo:posts -->

<!-- home page: only the 3 newest -->
<!-- hostilo:posts limit=3 -->
<!-- /hostilo:posts -->
```

Everything between the markers is replaced on every rebuild. Never edit it by hand.

## A post file: `_posts/<lang>/<slug>.html`

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
