# Hostilo Business site playbook

Rules for AI assistants that build and maintain a multilingual company website on [Hostilo](https://hostilo.app): folder structure, languages and `hreflang`, contact forms, news posts in a fixed template, site memory for the next person or AI, publishing through the Hostilo connector, redesigns without losing content, SEO, accessibility and a handover checklist. Plus a complete starter site.

Works with Claude (claude.ai and Claude Code), ChatGPT, Codex, Cursor, Windsurf, Gemini and any tool that reads `AGENTS.md`.

> Site memory (`_memory/`), news posts (`_posts/`), full-site snapshots and photo upload links are features of the **Hostilo Business** plan. All other rules apply to any Hostilo site, including the free plan.

## What is inside

| Path | What |
|---|---|
| [`hostilo-business-site/SKILL.md`](hostilo-business-site/SKILL.md) | the skill: 12 rules and a workflow for every typical task |
| [`hostilo-business-site/references/`](hostilo-business-site/references/) | languages, forms, site memory and posts, publishing, redesign, SEO / accessibility / performance, checklist |
| [`hostilo-business-site/starter/`](hostilo-business-site/starter/) | a complete starter site: Polish in the root, English in `/en/`, contact form, news list, site memory, post templates, one post |
| [`AGENTS.md`](AGENTS.md) | everything above in one file, for Codex, Cursor, ChatGPT Projects and other tools |
| [`hostilo-business-site.zip`](hostilo-business-site.zip) | the skill packaged for upload to claude.ai |

## Install

| Tool | How |
|---|---|
| **Claude (claude.ai)** | Settings → Capabilities → Skills → Upload skill → `hostilo-business-site.zip`. Connect Hostilo from the Claude connector directory. |
| **Claude Code** | copy `hostilo-business-site/` to `~/.claude/skills/` (personal) or `.claude/skills/` in your website project (team). |
| **ChatGPT** | create a Project, add `AGENTS.md` as a project file, and in the project instructions write: "Always follow the rules in AGENTS.md for the website <address> hosted on Hostilo." Enable the Hostilo app. |
| **Codex, Cursor, Windsurf** | put `AGENTS.md` in the root of your website project. |
| **Gemini** | create a Gem with the same instruction and `AGENTS.md` as knowledge. |

First prompt, for example: *"Build a company website for <name> in Polish and English following the Hostilo Business rules, based on the starter site. Company details: …"*

Then connect Hostilo (connector or REST API, see [hostilo.app/docs](https://hostilo.app/docs)) and the assistant can publish, add news posts and keep the site consistent for years.

## Starter site

Replace `https://example.com` with the site address (custom domain or `https://<name>.hostilo.page`) and `SITE_TOKEN` with the site's form token (shown by the Hostilo connector in `get_site`, or in the dashboard). Publish all files in one `publish_files` call or as a ZIP with `import_zip`.

## Polski

Zasady dla asystentów AI, którzy budują i prowadzą wielojęzyczną stronę firmową na [Hostilo](https://hostilo.app):
- struktura plików, języki i `hreflang`;
- formularz kontaktowy, aktualności w stałym wzorze;
- pamięć strony dla następnej osoby albo następnego AI;
- publikacja przez konektor, zmiana wyglądu bez utraty treści;
- SEO, dostępność i lista kontrolna przed oddaniem strony.

W pakiecie jest też gotowa strona startowa (PL + EN). Zasady są po angielsku, a asystent odpowiada w Twoim języku. Instalacja wygląda tak jak w tabeli powyżej. Pamięć strony, aktualności, migawki i link do zdjęć działają na planie Hostilo Business.

## Contributing and contact

Issues and pull requests are welcome. Questions: hello@hostilo.app.

License: MIT, © 2026 IMJ sp. z o.o.
