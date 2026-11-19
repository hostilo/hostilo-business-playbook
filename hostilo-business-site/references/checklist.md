# Checklist (live site, before handover and after every bigger change)

## Structure and links
- [ ] Every language has the same pages; menu and footer identical on all pages of a language.
- [ ] All internal links root-relative and clean (`/en/about`), no `.html`, no broken links (check the menu, footer, language switcher, buttons).
- [ ] Language switcher leads to the same page in the other language; current language marked.
- [ ] `/en` → `/en/` works; 404 page shows links to every language home.

## Head and SEO
- [ ] `<html lang>` correct on every page; unique title and description per page and language.
- [ ] Canonical and hreflang (all languages + x-default) with the final domain.
- [ ] Open Graph tags; JSON-LD Organization on the home page, NewsArticle in the post template.

## Forms
- [ ] Hostilo endpoint, correct `site_token`, `_honey` present and hidden, `email` and `message` required, labels, consent with privacy link.
- [ ] `_next` thank-you page per language exists and is on the same domain.
- [ ] Test message sent from the live site in each language and received by the owner.

## News (Business)
- [ ] `get_site` shows SITE.md, DESIGN.md, posts.json and templates without warnings.
- [ ] Lists contain `<!-- hostilo:posts -->` markers; newest post first; home page teaser limited.
- [ ] A test post with an image shows the image on the post page and in the list; RSS opens.

## Look and quality
- [ ] 375 px (phone) and desktop: no horizontal scroll, readable text, tap targets ≥ 44 px.
- [ ] Contrast ≥ 4.5:1, visible focus, images with alt text and width/height, WebP.
- [ ] No placeholder texts, no "lorem ipsum", no invented facts or reviews.
- [ ] Analytics only with consent (if GA4/GTM is used).

## Memory and handover
- [ ] `SITE.md` and `DESIGN.md` describe the current state (page map, integrations, decisions).
- [ ] Last publish had a `note`; the user knows the snapshot id of the last big change.
- [ ] Owner knows how to add a post: "Add a news post on <site> in all languages: …".
