# Redesign: a new look for the whole site

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
