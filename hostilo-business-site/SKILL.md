---
name: hostilo-business-site
description: Rules for building and maintaining a multilingual company website hosted on Hostilo (hostilo.app), especially on the Hostilo Business plan, where an AI assistant runs the site for years through the Hostilo connector. Use whenever you build, migrate, redesign or edit a Hostilo site, add news posts, translate pages, add a contact form, or prepare a site for handover. Triggers on: Hostilo, hostilo.page, Hostilo Business, publish_files, get_site, _memory, _posts, site memory, news post, add a post, contact form on Hostilo, multilingual static site, migrate from WordPress to Hostilo.
---

# Hostilo Business site playbook

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
